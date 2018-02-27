---
title: "Tworzenie zasobów do używania z usługą Azure Site Recovery | Microsoft Docs"
description: "Dowiedz się, jak przygotować platformę Azure do replikacji maszyn lokalnych przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: d1aadd6b44d64f0bdb35ea02d628bedfc366ad3c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Przygotowywanie zasobów platformy Azure do replikacji maszyn lokalnych

Usługa [Azure Site Recovery](site-recovery-overview.md) przyczynia się do strategii związanej z ciągłością biznesową i odzyskiwaniem po awarii (BCDR, business continuity and disaster recovery) przez zapewnienie niezawodnego działania aplikacji biznesowych podczas planowanych lub nieplanowanych przestojów. Usługa Site Recovery zarządza odzyskiwaniem po awarii maszyn lokalnych i maszyn wirtualnych platformy Azure, a także organizuje to odzyskiwanie. Obejmuje to replikację, przechodzenie w tryb failover i odzyskiwanie.

W tym samouczku przedstawiono sposób przygotowania składników platformy Azure w przypadku replikowania lokalnych maszyn wirtualnych (korzystających z funkcji Hyper-V lub oprogramowania VMware) bądź serwerów fizycznych z systemami Windows i Linux do platformy Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Sprawdzanie, czy konto ma uprawnienia do replikacji
> * Utworzenie konta magazynu platformy Azure.
> * Konfigurowanie sieci platformy Azure. Gdy maszyny wirtualne są tworzone po przejściu w tryb failover, dołączają one do sieci platformy Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="verify-account-permissions"></a>Sprawdzanie uprawnień konta

Jeśli bezpłatne konto platformy Azure zostało właśnie utworzone, jesteś administratorem subskrypcji. Jeśli nie jesteś administratorem subskrypcji, współpracuj z administratorem w celu przypisania potrzebnych uprawnień. Aby włączyć replikację dla nowej maszyny wirtualnej, konieczne jest:

- Uprawnienie do tworzenia maszyny wirtualnej w wybranej grupie zasobów
- Uprawnienie do tworzenia maszyny wirtualnej w wybranej sieci wirtualnej
- Uprawnienie do zapisu w wybranym koncie magazynu

Wbudowana rola „Współautor maszyny wirtualnej” ma te uprawnienia. Musisz mieć również uprawnienie do zarządzania operacjami usługi Azure Site Recovery. Rola „Współautor usługi Site Recovery” ma wszystkie uprawnienia wymagane do zarządzania operacjami usługi Site Recovery w magazynie usługi Recovery Services.

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Obrazy replikowanych maszyn są przechowywane w usłudze Azure Storage. Maszyny wirtualne Azure są tworzone na podstawie magazynu w momencie przejścia w tryb failover ze środowiska lokalnego do platformy Azure.

1. W menu witryny [Azure Portal](https://portal.azure.com) kliknij pozycję **Nowy** -> **Magazyn** -> **Konto magazynu**.
2. W obszarze **Tworzenie konta magazynu** wprowadź nazwę konta. Na potrzeby tego samouczka używana będzie nazwa **contosovmsacct1910171607**. Nazwa musi mieć od 3 do 24 znaków długości, zawierać wyłącznie cyfry i małe litery oraz musi być unikatowa w obrębie platformy Azure.
3. Użyj modelu wdrażania usługi **Resource Manager**.
4. Wybierz pozycję **Ogólnego przeznaczenia** > **Standardowy**. Nie wybieraj magazynu obiektów blob.
5. Wybierz domyślną wartość **RA-GRS** na potrzeby nadmiarowości magazynu.
6. Wybierz subskrypcję, w ramach której chcesz utworzyć nowe konto magazynu.
7. Określ nową grupę zasobów. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Na potrzeby tych samouczków używana jest nazwa **ContosoRG**.
8. Wybierz lokalizację geograficzną dla swojego konta magazynu. Konto magazynu musi znajdować się w tym samym regionie, co magazyn usługi Recovery Services. Na potrzeby tych samouczków używany jest region **Europa Zachodnia**.

   ![create-storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Kliknij pozycję **Utwórz**, aby utworzyć konto magazynu.

## <a name="create-a-vault"></a>Tworzenie magazynu

1. W witrynie Azure Portal kliknij pozycję **Utwórz zasób** > **Monitorowanie i zarządzanie** > **Usługi Backup i Site Recovery**.
2. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Na potrzeby tego samouczka używana jest nazwa **ContosoVMVault**.
3. Wybierz istniejącą grupę zasobów o nazwie **contosoRG**.
4. Określ region platformy Azure **Europa Zachodnia**, który jest używany w tym zestawie samouczków.
5. Aby szybko uzyskać dostęp do magazynu z pulpitu nawigacyjnego, kliknij pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz**.

   ![Nowy magazyn](./media/tutorial-prepare-azure/new-vault-settings.png)

   Nowy magazyn zostanie wyświetlony w sekcji **Pulpit nawigacyjny** > **Wszystkie zasoby** oraz na głównej stronie **magazynów usługi Recovery Services**.

## <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure

Gdy maszyny wirtualne platformy Azure są tworzone na podstawie magazynu po przejściu w tryb failover, dołączają one do tej sieci.

1. W witrynie [Azure Portal](https://portal.azure.com) kliknij pozycję **Utwórz zasób** > **Sieć** > **Sieć wirtualna**.
2. Jako model wdrażania pozostaw wybraną pozycję **Resource Manager**. Wdrażanie przy użyciu usługi Resource Manager jest preferowanym modelem wdrażania.
   - Określ nazwę sieci. Nazwa musi być unikatowa w obrębie grupy zasobów platformy Azure. Zostanie użyta nazwa **ContosoASRnet**
   - Użyj istniejącej grupy zasobów **contosoRG**.
   - Określ zakres adresów sieci 10.0.0.0/24.
   - W tym samouczku podsieć nie jest wymagana.
   - Wybierz subskrypcję, w ramach której chcesz utworzyć sieć.
   - Wybierz lokalizację **Europa Zachodnia**. Sieć musi znajdować się w tym samym regionie co magazyn usługi Recovery Services.
3. Kliknij przycisk **Utwórz**.

   ![create-network](media/tutorial-prepare-azure/create-network.png)

   Utworzenie sieci wirtualnej zajmuje kilka sekund. Po utworzeniu sieć jest wyświetlana na pulpicie nawigacyjnym witryny Azure Portal.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Przygotowywanie lokalnej infrastruktury VMware do odzyskiwania po awarii na platformie Azure](tutorial-prepare-on-premises-vmware.md)
