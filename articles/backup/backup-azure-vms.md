---
title: "Utwórz kopię zapasową wdrożone w klasycznej maszyn wirtualnych platformy Azure do magazynu kopii zapasowych | Dokumentacja firmy Microsoft"
description: "Odnajdowanie, rejestrowania i utworzyć kopię zapasową maszyn wirtualnych z tych procedur do utworzenia kopii zapasowej maszyny wirtualnej platformy Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: kopii zapasowej maszyny wirtualnej. Tworzenie kopii zapasowej maszyny wirtualnej; Kopia zapasowa i odzyskiwanie po awarii; kopii zapasowej maszyny wirtualnej
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: ca3fb650a133efb67c0ad2cd96847c6a0e21c876
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a>Tworzenie kopii zapasowej maszyn wirtualnych platformy Azure (klasyczny portal)
> [!div class="op_single_selector"]
> * [Wykonaj kopię zapasową maszyn wirtualnych na magazyn usług odzyskiwania](backup-azure-arm-vms.md)
> * [Kopii zapasowych maszyn wirtualnych do magazynu kopii zapasowej](backup-azure-vms.md)
>
>

Ten artykuł zawiera procedury wykonywania kopii zapasowych wdrożone klasycznym Azure maszynę wirtualną (VM) do magazynu kopii zapasowej. Istnieje kilka zadań, które trzeba zajmie się przed utworzeniem kopii zapasowej maszyny wirtualnej platformy Azure. Jeśli nie zostało to jeszcze zrobione, wykonaj [wymagania wstępne](backup-azure-vms-prepare.md) do przygotowania środowiska do tworzenia kopii zapasowych maszyn wirtualnych.

Aby uzyskać dodatkowe informacje, zobacz artykuły w [planowania infrastruktury kopii zapasowych maszyn wirtualnych na platformie Azure](backup-azure-vms-introduction.md) i [maszyn wirtualnych platformy Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).

> [!NOTE]
> Platforma Azure ma dwa modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [Resource Manager i model klasyczny](../azure-resource-manager/resource-manager-deployment-model.md). Magazyn kopii zapasowych może chronić tylko wdrożone w klasycznej maszyn wirtualnych. Nie można chronić wdrożonych przez Menedżera zasobów maszyn wirtualnych w magazynie kopii zapasowej. Zobacz [kopii zapasowych maszyn wirtualnych w magazynie usług odzyskiwania](backup-azure-arm-vms.md) szczegółowe informacje na temat pracy z usług odzyskiwania magazynów.
>
>

Tworzenie kopii zapasowych maszyn wirtualnych platformy Azure obejmuje trzy kroki klucza:

![Trzy kroki, aby utworzyć kopię zapasową maszyny Wirtualnej platformy Azure IaaS](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> Tworzenie kopii zapasowych maszyn wirtualnych jest procesem lokalnym. Nie można utworzyć kopii zapasowych maszyn wirtualnych w jednym regionie do magazynu kopii zapasowych w innym regionie. Dlatego należy utworzyć magazyn kopii zapasowych w każdym regionie Azure, gdy istnieją maszyny wirtualne, które zostaną umieszczone w kopii zapasowej.
>
> [!IMPORTANT]
> Począwszy od marca 2017 r. nie można już tworzyć magazynów kopii zapasowych za pomocą klasycznego portalu.
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> Po 30 listopada 2017 nie można utworzyć magazyny kopii zapasowych przy użyciu programu PowerShell. **W dniu 30 listopada 2017**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
>

## <a name="step-1---discover-azure-virtual-machines"></a>Krok 1 — odnajdywanie maszyn wirtualnych platformy Azure
Aby upewnić się, że żadnych nowych maszyn wirtualnych (VM) dodane do subskrypcji są identyfikowane przed zarejestrowaniem, uruchomić proces odnajdowania. Proces wyszukuje na platformie Azure listę maszyn wirtualnych w ramach subskrypcji wraz z dodatkowymi informacjami, takimi jak nazwa usługi w chmurze i region.

1. Zaloguj się do [klasyczny portal](http://manage.windowsazure.com/)
2. Na liście usług Azure, kliknij **usług odzyskiwania** aby otworzyć listę magazynów kopii zapasowych i odzyskiwania lokacji.
    ![Otwórz magazyn listy](./media/backup-azure-vms/choose-vault-list.png)
3. Na liście magazyny kopii zapasowych wybierz magazyn, aby utworzyć kopię zapasową maszyny Wirtualnej.

    Jeśli jest to nowy magazyn do otwartego portalu **Szybki Start** strony.

    ![Otwórz zarejestrowane elementy menu](./media/backup-azure-vms/vault-quick-start.png)

    Jeśli wcześniej skonfigurowano Magazyn, otwartego portalu do ostatnio używanych menu.
4. W menu magazynu (w górnej części strony), kliknij przycisk **zarejestrowane elementy**.

    ![Otwórz zarejestrowane elementy menu](./media/backup-azure-vms/vault-menu.png)
5. Z menu **Typ** wybierz polecenie **Maszyna wirtualna platformy Azure**.

    ![Wybór obciążenia](./media/backup-azure-vms/discovery-select-workload.png)
6. Kliknij pozycję **ODNAJDŹ** w dolnej części strony.
    ![Przycisk Odnajdź](./media/backup-azure-vms/discover-button-only.png)

    Proces odnajdywania może zająć kilka minut, gdy maszyny wirtualne są wyszczególniane. W dolnej części ekranu znajduje się powiadomienie informujące, że proces jest uruchomiony.

    ![Odnajdywanie maszyn wirtualnych](./media/backup-azure-vms/discovering-vms.png)

    Powiadomienie ulega zmianie, gdy proces zostanie zakończony. Jeśli proces odnajdowania nie znaleziono maszyn wirtualnych, najpierw upewnij się, że istnieje maszyn wirtualnych. Jeśli istnieje maszyn wirtualnych, upewnij się, że maszyny wirtualne znajdują się w tym samym regionie co magazyn kopii zapasowych. Jeśli maszyny wirtualne istnieją i są w tym samym regionie, upewnij się, że maszyny wirtualne nie są już zarejestrowane do magazynu kopii zapasowych. Jeśli maszyna wirtualna jest przypisany do magazynu kopii zapasowych nie jest dostępne do przypisania do innych magazynów kopii zapasowych.

    ![Odnajdywanie zostało zakończone](./media/backup-azure-vms/discovery-complete.png)

    Gdy nowe elementy zostały odnalezione, przejdź do kroku 2 i zarejestruj maszyny wirtualne.

## <a name="step-2---register-azure-virtual-machines"></a>Krok 2 — rejestrowanie maszyn wirtualnych platformy Azure
Możesz zarejestrować maszyny wirtualnej platformy Azure, aby skojarzyć ją z usługą kopia zapasowa Azure. Jest to zazwyczaj jednorazowe działania.

1. Przejdź do magazynu kopii zapasowych w obszarze **usług odzyskiwania** w portalu Azure, a następnie kliknij przycisk **zarejestrowane elementy**.
2. Z menu rozwijanego wybierz pozycję **Maszyna wirtualna platformy Azure**.

    ![Wybieranie obciążenia](./media/backup-azure-vms/discovery-select-workload.png)
3. Kliknij pozycję **ZAREJESTRUJ** w dolnej części strony.
    ![Przycisk Zarejestruj](./media/backup-azure-vms/register-button-only.png)
4. W menu skrótów **Zarejestruj elementy** wybierz maszyny wirtualne, które chcesz zarejestrować. W przypadku co najmniej dwie maszyny wirtualne o takiej samej nazwie, użyj usługi w chmurze do rozróżniania między nimi.

   > [!TIP]
   > W tym samym czasie można zarejestrować wiele maszyn wirtualnych.
   >
   >

    Zadanie jest tworzone dla każdej maszyny wirtualnej, która zostanie wybrana.
5. W powiadomieniu kliknij pozycję **Wyświetl zadanie**, aby przejść do strony **Zadania**.

    ![Rejestrowanie zadania](./media/backup-azure-vms/register-create-job.png)

    Maszyna wirtualna pojawia się również na liście zarejestrowanych elementów wraz ze stanem operacji rejestrowania.

    ![Rejestrowanie — stan 1](./media/backup-azure-vms/register-status01.png)

    Po zakończeniu operacji stan zostanie zmieniony w celu odzwierciedlenia stanu oznaczającego *zarejestrowanie*.

    ![Rejestrowanie — stan 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a>Krok 3 — ochrona maszyn wirtualnych platformy Azure
Teraz można skonfigurować zasady tworzenia kopii zapasowej i przechowywania dla maszyny wirtualnej. Wiele maszyn wirtualnych mogą być chronione przy użyciu pojedynczej chronić akcji.

Azure magazyny kopii zapasowych utworzonych po 2015 może pochodzić z domyślnych zasad wbudowany w magazynie. Ta zasada domyślna zawiera zachowanie domyślne, 30 dni i harmonogram tworzenia kopii zapasowych raz dziennie.

1. Przejdź do magazynu kopii zapasowych w obszarze **usług odzyskiwania** w portalu Azure, a następnie kliknij przycisk **zarejestrowane elementy**.
2. Z menu rozwijanego wybierz pozycję **Maszyna wirtualna platformy Azure**.

    ![Wybieranie obciążenia w portalu](./media/backup-azure-vms/select-workload.png)
3. Kliknij pozycję **CHROŃ** w dolnej części strony.

    **Kreator ochrony elementów** pojawi się. Kreator wyświetla tylko maszyny wirtualne, które są zarejestrowane i nie są chronione. Wybierz maszyny wirtualne, które chcesz chronić.

    Jeśli istnieje co najmniej dwie maszyny wirtualne o takiej samej nazwie, użyj usługi w chmurze do rozróżniania między maszynami wirtualnymi.

   > [!TIP]
   > Można chronić wiele maszyn wirtualnych w tym samym czasie.
   >
   >

    ![Konfigurowanie ochrony w dużej skali](./media/backup-azure-vms/protect-at-scale.png)

4. Wybierz **harmonogram tworzenia kopii zapasowych** do tworzenia kopii zapasowych maszyn wirtualnych, które zostały wybrane. Można wybierać z istniejącego zestawu zasad lub zdefiniuj nowy.

    Z jednymi zasadami tworzenia kopii zapasowych może być skojarzonych wiele maszyn wirtualnych. Jednak maszynę wirtualną można skojarzyć tylko z jednymi zasadami na dowolnym etapie w czasie.

    ![Ochrona za pomocą nowych zasad](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Zasady tworzenia kopii zapasowych obejmują schemat przechowywania dla zaplanowanych kopii zapasowych. W przypadku wybrania istniejących zasad tworzenia kopii zapasowej nie można zmodyfikować opcje przechowywania w następnym kroku.
   >
   >

5. Wybierz **zakres przechowywania** do skojarzenia z kopii zapasowych.

    ![Chroń za pomocą elastycznych przechowywania](./media/backup-azure-vms/policy-retention.png)

    Zasady przechowywania określają czas przechowywania kopii zapasowej. Możliwe jest określenie różnych zasad przechowywania na podstawie czasu tworzenia kopii zapasowej. Na przykład punktu kopii zapasowej pobierane raz dziennie (która służy jako punkt odzyskiwania operacyjne) mogą zostać zachowane przez 90 dni. Z kolei punktu kopii zapasowej są pod koniec każdego kwartału (na potrzeby inspekcji) może być konieczne przechowywane przez wiele miesięcy lub lat.

    ![Kopia zapasowa maszyny wirtualnej jest tworzona z punktem odzyskiwania](./media/backup-azure-vms/long-term-retention.png)

    W tym obrazie przykładzie:

   * **Zasady przechowywania codziennych**: kopii zapasowych wykonanych codziennie są przechowywane przez 30 dni.
   * **Co tydzień zasady przechowywania**: kopii zapasowych wykonanych co tydzień w niedzielę zostaną zachowane w celu 104 tygodni.
   * **Miesięczne zasady przechowywania**: kopie zapasowe wykonane w niedzielę ostatniego dnia każdego miesiąca są zachowywane przez 120 miesięcy.
   * **Zasady przechowywania corocznych**: kopie zapasowe wykonane w pierwszym niedziela co stycznia zostaną zachowane w celu 99 lat.

     Aby skonfigurować zasady ochrony i skojarzyć maszyn wirtualnych w tym zasadom dla każdej maszyny wirtualnej, która zostanie wybrana tworzone jest zadanie.
6. Aby wyświetlić listę **konfiguracji ochrony** zadań z menu magazynów kliknij **zadania** i wybierz **konfiguracji ochrony** z **operacji** filtru.

    ![Zadanie konfigurowania ochrony](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a>Początkowa kopia zapasowa
Gdy maszyna wirtualna jest chroniona za pomocą zasad, jest wyświetlane w obszarze **chronione elementy** kartę ze statusem *chroniona — (oczekiwanie początkowa kopia zapasowa)*. Domyślnie pierwszą zaplanowaną kopią zapasową jest *początkowa kopia zapasowa*.

Aby wyzwolić tworzenie początkowej kopii zapasowej natychmiast po skonfigurowaniu ochrony:

1. W dolnej części **chronione elementy** kliknij przycisk **Utwórz kopię zapasową teraz**.

    Usługa Azure Backup tworzy zadanie dla operacji tworzenia początkowej kopii zapasowej.
2. Kliknij kartę **Zadania**, aby wyświetlić listę zadań.

    ![Tworzenie kopii zapasowej w toku](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> Podczas tworzenia kopii zapasowej usługa Azure Backup wystawia polecenie zapasowy numer wewnętrzny w każdej maszyny wirtualnej, aby opróżnić wszystkie zadania zapisu i migawki spójne.
>
>

Po zakończeniu tworzenia początkowej kopii zapasowej, stan maszyny wirtualnej w **chronione elementy** jest karta *chronione*.

![Kopia zapasowa maszyny wirtualnej jest tworzona z punktem odzyskiwania](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a>Wyświetlanie stanu kopii zapasowej i szczegóły
Gdy chroniony, liczba maszyn wirtualnych zwiększa także w **pulpitu nawigacyjnego** strony podsumowania. **Pulpitu nawigacyjnego** strony zawiera również liczbę zadań w ciągu ostatnich 24 godzin, które zostały *pomyślnie*, ma *nie powiodło się*i są *w toku*. Na **zadania** użyj **stan**, **operacji**, lub **z** i **do** menu do filtrowania zadań.

![Stan tworzenia kopii zapasowej na stronie pulpitu nawigacyjnego](./media/backup-azure-vms/dashboard-protectedvms.png)

Wartości na pulpicie nawigacyjnym są odświeżane co 24 godziny.

## <a name="troubleshooting-errors"></a>Rozwiązywanie problemów z błędami
Jeśli wystąpiły problemy podczas tworzenia kopii zapasowej maszyny wirtualnej przyjrzeć się [maszyny Wirtualnej artykuł dotyczący rozwiązywania problemów](backup-azure-vms-troubleshoot.md) Aby uzyskać pomoc.

## <a name="next-steps"></a>Następne kroki
* [Monitorowanie maszyn wirtualnych i zarządzanie nimi](backup-azure-manage-vms.md)
* [Przywracanie maszyn wirtualnych](backup-azure-restore-vms.md)
