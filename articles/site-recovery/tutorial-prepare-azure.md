---
title: "Utwórz zasoby do użycia z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przygotować Azure do replikację maszyn lokalnych przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 71d740107eb2082e3f112941e1d4abd715d25807
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Przygotowanie zasobów Azure dla replikację maszyn lokalnych

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do ciągłości i odzyskiwaniem po awarii (BCDR) odzyskiwania strategią biznesową, zatrzymując aplikacji biznesowych i uruchamiając dostępne podczas planowanych lub nieplanowanych przestojów. Usługa Site Recovery zarządza i organizuje odzyskiwania po awarii maszyn lokalnych i maszyn wirtualnych platformy Azure (maszyny wirtualne), łącznie z replikacji, trybu failover i odzyskiwania.

Ten samouczek pokazuje, jak przygotować składniki platformy Azure, jeśli chcesz replikować do platformy Azure VMs lokalnymi (Hyper-V lub programu VMware) lub serwerach fizycznych systemu Windows i Linux. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Sprawdź, czy konto ma uprawnienia do replikacji
> * Utworzenie konta magazynu platformy Azure.
> * Ustaw sieci platformy Azure. Podczas tworzenia maszyn wirtualnych Azure po w tryb failover, jest częścią tej sieci platformy Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="verify-account-permissions"></a>Sprawdź uprawnienia konta

Jeśli właśnie utworzony bezpłatne konto platformy Azure, użytkownik jest administratorem subskrypcji. Jeśli nie jesteś administratorem subskrypcji, współpracować z administratorem, aby przypisać uprawnienia, które są potrzebne. Aby włączyć replikację dla nowej maszyny wirtualnej, musi być:

- Uprawnienia do tworzenia maszyny Wirtualnej w wybranej grupy zasobów
- Uprawnienia do tworzenia maszyny Wirtualnej w wybranej sieci wirtualnej
- Uprawnienia do zapisu do wybranego konta magazynu

Wbudowane roli "Współautor maszyny wirtualnej" ma te uprawnienia. Należy również uprawnienia do zarządzania operacjami usługi Azure Site Recovery. Roli "Współautor odzyskiwania lokacji" ma wszystkie uprawnienia wymagane do zarządzania operacjami usługi Site Recovery w magazynie usług odzyskiwania.

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Obrazy maszyn replikowanych są przechowywane w magazynie Azure. Maszyny wirtualne platformy Azure są tworzone z magazynu, jeśli możesz przełączyć się z lokalnymi na platformie Azure.

1. W [portalu Azure](https://portal.azure.com) menu, kliknij przycisk **nowy** -> **magazynu** -> **konta magazynu**.
2. Wprowadź nazwę konta magazynu. Te samouczki używamy nazwy **contosovmsacct1910171607**. Nazwa musi być unikatowa w obrębie platformy Azure i należeć do zakresu od 3 do 24 znaków, cyfry i małe litery.
3. Użyj **Resource Manager** modelu wdrażania.
4. Wybierz **ogólnego przeznaczenia** > **standardowe**.
5. Wybierz domyślną **RA-GRS** nadmiarowości magazynu.
6. Wybierz subskrypcję, w ramach której chcesz utworzyć nowe konto magazynu.
7. Określ nową grupę zasobów. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Te samouczki używamy nazwy **ContosoRG**.
8. Wybierz lokalizację geograficzną dla swojego konta magazynu. Konto magazynu musi być w tym samym regionie co magazyn usług odzyskiwania. Te samouczki używamy **Europa** regionu.

   ![Utwórz storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Kliknij pozycję **Utwórz**, aby utworzyć konto magazynu.

## <a name="create-a-vault"></a>Tworzenie magazynu

1. W menu portalu Azure kliknij **nowy** > **monitorowanie i zarządzanie** >
    **kopii zapasowych i odzyskiwania lokacji**.
2. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. W tym samouczku używamy **ContosoVMVault**.
3. Wybierz istniejącą grupę zasobów o nazwie **contosoRG**.
4. Określ region platformy Azure **Europa**, które firma Microsoft korzysta z tego zestawu samouczki.
5. Aby szybko uzyskać dostęp do magazynu z pulpitu nawigacyjnego, kliknij przycisk **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Nowy magazyn](./media/tutorial-prepare-azure/new-vault-settings.png)

   Nowy magazyn będzie wyświetlany na **pulpitu nawigacyjnego** > **wszystkie zasoby**i w głównym **Magazyny usług odzyskiwania** strony.

## <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure

Po utworzeniu maszyn wirtualnych platformy Azure z magazynu po pracy awaryjnej jest częścią tej sieci.

1. W [portalu Azure](https://portal.azure.com) menu, kliknij przycisk **nowy** > **sieci** >
    **sieci wirtualnej**
2. Pozostaw **Resource Manager** wybrany jako model wdrażania. Menedżer zasobów jest model preferowany wdrożenia.
   - Określ nazwę sieci. Nazwa musi być unikatowa w obrębie grupy zasobów platformy Azure. Używamy nazwy **ContosoASRnet**
   - Użyj istniejącej grupy zasobów **contosoRG**.
   - Określ sieć zakresu adresów 10.0.0.0/24.
   - W tym samouczku, nie musisz podsieci.
   - Wybierz subskrypcję, w którym ma zostać utworzony w sieci.
   - Wybierz lokalizację **Europa**. Sieć musi znajdować się w tym samym regionie co magazyn Usług odzyskiwania.
3. Kliknij przycisk **Utwórz**.

   ![Utwórz sieć](media/tutorial-prepare-azure/create-network.png)

   Sieć wirtualna ma kilka sekund, aby utworzyć. Po utworzeniu, sprawdzić w pulpicie nawigacyjnym portalu Azure.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Przygotowywanie lokalnej infrastruktury VMware do odzyskiwania awaryjnego na platformie Azure](tutorial-prepare-on-premises-vmware.md)
