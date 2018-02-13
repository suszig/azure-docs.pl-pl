---
title: "Migrowanie maszyn wirtualnych Azure między regiony platformy Azure przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Użyj usługi Azure Site Recovery do migracji maszyn wirtualnych IaaS platformy Azure z jednego regionu Azure, do drugiego."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: e7b925d2daed11ee4e070cda6bcbd4a3511d9c17
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="migrate-azure-vms-to-another-region"></a>Migrowanie maszyn wirtualnych platformy Azure do innego regionu

Oprócz używania [usługi Azure Site Recovery](site-recovery-overview.md) usługi do zarządzania i organizowania odzyskiwania po awarii maszyn lokalnych i maszyn wirtualnych platformy Azure na potrzeby ciągłość prowadzenia działalności biznesowej i odzyskiwania po awarii (BCDR), możesz również użyć witryny Odzyskiwanie do zarządzania migracji maszyn wirtualnych platformy Azure w regionie pomocniczym. Aby dokonać migracji maszyn wirtualnych platformy Azure, włączyć replikację dla nich, a przełączyć je od regionu podstawowego w regionie dodatkowym wybranych przez użytkownika.

Ten samouczek przedstawia sposób migracji maszyn wirtualnych platformy Azure do innego regionu. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie magazynu usług odzyskiwania
> * Włącz replikację dla maszyny Wirtualnej
> * Tryb failover, aby przeprowadzić migrację maszyny Wirtualnej

Ten samouczek zakłada się, że masz już subskrypcję platformy Azure. Jeśli nie zostanie utworzona [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/) przed rozpoczęciem.

>[!NOTE]
>
> Replikacja odzyskiwania lokacji dla maszyn wirtualnych platformy Azure jest obecnie w przeglądzie.



## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka należy maszynach wirtualnych platformy Azure w regionie Azure, z którego chcesz przeprowadzić migrację. Ponadto istnieje wiele ustawień, które należy sprawdzić przed jego uruchomieniem.


### <a name="verify-target-resources"></a>Sprawdź zasoby obiektów docelowych

1. Sprawdź, czy subskrypcji platformy Azure umożliwia tworzenie maszyn wirtualnych w regionie docelowej służącej do odzyskiwania po awarii. Skontaktuj się z pomocą techniczną, aby włączyć wymagane przydziału.

2. Upewnij się, że subskrypcja ma za mało zasobów do obsługi maszyn wirtualnych o rozmiarze zgodne źródłowe maszyny wirtualne. Usługa Site Recovery wybiera ten sam rozmiar lub najbliższy rozmiar możliwe w dla docelowej maszyny Wirtualnej.


### <a name="verify-account-permissions"></a>Sprawdź uprawnienia konta

Jeśli właśnie utworzony bezpłatne konto platformy Azure jesteś administratorem subskrypcji. Jeśli nie jesteś administratorem subskrypcji, współpracować z administratorem, aby przypisać uprawnienia, które są potrzebne. Aby włączyć replikację dla nowej maszyny Wirtualnej, musi być:

1. Uprawnienia do tworzenia maszyny Wirtualnej na zasobów platformy Azure. Wbudowane roli "Współautor maszyny wirtualnej" ma te uprawnienia, które obejmują:
    - Uprawnienia do tworzenia maszyny Wirtualnej w wybranej grupy zasobów
    - Uprawnienia do tworzenia maszyny Wirtualnej w wybranej sieci wirtualnej
    - Uprawnienia do zapisu do wybranego konta magazynu

2. Należy również uprawnienia do zarządzania operacjami usługi Azure Site Recovery. Roli "Współautor odzyskiwania lokacji" ma wszystkie uprawnienia wymagane do zarządzania operacjami usługi Site Recovery w magazynie usług odzyskiwania.


### <a name="verify-vm-outbound-access"></a>Sprawdź wychodzący dostęp do maszyny Wirtualnej

1. Upewnij się, że nie używasz uwierzytelniania serwera proxy do kontrolowania łączność sieciową dla maszyn wirtualnych, które chcesz migrować. 
2. Do celów tego samouczka przyjęto założenie, że maszyn wirtualnych chcesz przeprowadzić migrację można uzyskać dostęp do Internetu oraz nie jest używany serwer proxy zapory w celu kontrolowania dostępu ruchu wychodzącego. Jeśli jest, sprawdź wymagania [tutaj](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

### <a name="verify-vm-certificates"></a>Sprawdź certyfikaty maszyny Wirtualnej

Sprawdź, czy wszystkie najnowsze certyfikaty główne są dostępne na maszynach wirtualnych platformy Azure, które chcesz migrować. Jeśli nie są najnowsze certyfikaty główne, maszyny Wirtualnej nie można zarejestrować do odzyskiwania lokacji z powodu ograniczeń zabezpieczeń.

- Dla maszyn wirtualnych systemu Windows należy zainstalować wszystkie najnowsze aktualizacje systemu Windows na Maszynie wirtualnej, tak aby były wszystkich zaufanych certyfikatów głównych na komputerze. W środowisku bez połączenia wykonaj standardowe usługi Windows Update i procesy aktualizacji certyfikatów dla Twojej organizacji.
- Dla maszyn wirtualnych systemu Linux postępuj zgodnie ze wskazówkami, w dostarczonych przez użytkownika dystrybutor systemu Linux, aby uzyskać najnowsze zaufanych certyfikatów głównych i listy odwołania certyfikatów na maszynie Wirtualnej.



## <a name="create-a-vault"></a>Tworzenie magazynu

Utwórz magazyn w dowolnym regionie, z wyjątkiem region źródła.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Kliknij pozycję **Nowy** > **Monitorowanie i zarządzanie** > **Backup and Site Recovery**.
3. W **nazwa**, określ przyjazną nazwę **ContosoVMVault**. Jeśli masz więcej niż jedną subskrypcję, wybierz odpowiedni.
4. Utwórz grupę zasobów **ContosoRG**.
5. Określ region platformy Azure. Aby sprawdzić obsługiwane regiony, zobacz sekcję dotyczącą dostępności geograficznej w temacie [Szczegóły cennika usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Aby szybko uzyskać dostęp do magazynu z pulpitu nawigacyjnego, kliknij przycisk **Przypnij do pulpitu nawigacyjnego** , a następnie kliknij przycisk **Utwórz**.

   ![Nowy magazyn](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

Nowy magazyn zostanie dodany do **pulpitu nawigacyjnego** w obszarze **wszystkie zasoby**i w głównym **Magazyny usług odzyskiwania** strony.






## <a name="select-the-source"></a>Wybierz źródło

1. W Magazyny usług odzyskiwania kliknij **ConsotoVMVault** > **+ Replikuj**.
2. W **źródła**, wybierz pozycję **Azure — wersja ZAPOZNAWCZA**.
3. W **lokalizacja źródłowa**, wybierz źródło region platformy Azure, w którym są uruchomione maszyny wirtualne.
4. Wybierz model wdrażania Menedżera zasobów. Następnie wybierz **źródłowa grupa zasobów**.
5. Kliknij pozycję **OK**, aby zapisać ustawienia.


## <a name="enable-replication-for-azure-vms"></a>Włącz replikację maszyn wirtualnych platformy Azure

Usługa Site Recovery pobiera listę maszyn wirtualnych, skojarzone z subskrypcji i grupy zasobów.


1. W portalu Azure kliknij **maszyn wirtualnych**.
2. Wybierz chcesz przeprowadzić migrację maszyny Wirtualnej. Następnie kliknij przycisk **OK**.
3. W **ustawienia**, kliknij przycisk **odzyskiwania po awarii (wersja zapoznawcza)**.
4. W **konfiguracji odzyskiwania po awarii** > **region docelowy** wybierz region docelowego, do którego będą replikowane.
5. W tym samouczku Zaakceptuj ustawienia domyślne.
6. Kliknij przycisk **włączyć replikację**. Spowoduje to uruchomienie zadania, aby włączyć replikację dla maszyny Wirtualnej.

    ![Włączanie replikacji](media/tutorial-migrate-azure-to-azure/settings.png)

>[!NOTE]
  >
  > Obecnie replikacja maszyn wirtualnych Azure z dyskami zarządzanego nie jest obsługiwana. 

## <a name="run-a-failover"></a>Uruchamianie trybu failover

1. W **ustawienia** > **elementy replikowane**, kliknij komputer, a następnie kliknij **pracy awaryjnej**.
2. W **pracy awaryjnej**, wybierz pozycję **najnowsze**. Ustawienie klucza szyfrowania nie jest odpowiedni dla tego scenariusza.
3. Wybierz **Zamknij maszynę przed rozpoczęciem pracy awaryjnej**. Usługa Site Recovery próbuje zamknąć źródłowej maszyny Wirtualnej, aby mogło nastąpić wyzwolenie pracy awaryjnej. Tryb failover trwa nawet w przypadku zamknięcia nie powiedzie się. Możesz śledzić postęp trybu failover **zadania** strony.
4. Sprawdź, czy maszyny Wirtualnej Azure jest wyświetlany na platformie Azure, zgodnie z oczekiwaniami.
5. W **elementy replikowane**, kliknij prawym przyciskiem myszy maszyny Wirtualnej > **przeprowadzenia migracji**. To kończy proces migracji i zatrzymuje replikacji dla maszyny Wirtualnej.



## <a name="next-steps"></a>Kolejne kroki

W tym samouczku zostały zmigrowane maszyny Wirtualnej platformy Azure w innym regionie Azure. Teraz możesz skonfigurować odzyskiwania po awarii dla migrowanych maszyny Wirtualnej.

> [!div class="nextstepaction"]
> [Konfigurowanie odzyskiwania po awarii po migracji](azure-to-azure-quickstart.md)

