---
title: "Pierwsze spojrzenie — tworzenie kopii zapasowej maszyn wirtualnych platformy Azure przy użyciu magazynu kopii zapasowych | Microsoft Docs"
description: "Użyj portalu klasycznego do tworzenia kopii zapasowych maszyn wirtualnych platformy Azure w magazynie kopii zapasowych. W tym samouczku wyjaśniono wszystkie etapy, m.in. tworzenie magazynu kopii zapasowych, rejestrowanie maszyn wirtualnych, tworzenie zasad kopii zapasowych i uruchamianie początkowego zadania tworzenia kopii zapasowej."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 3/10/2017
ms.author: markgal;
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 8883ff1601c521d05068452b1b58cadaee1a941f
ms.lasthandoff: 03/14/2017


---
# <a name="first-look-backing-up-azure-virtual-machines"></a>Pierwsze spojrzenie: tworzenie kopii zapasowych maszyn wirtualnych platformy Azure
> [!div class="op_single_selector"]
> * [Ochrona maszyn wirtualnych przy użyciu magazynu usługi Recovery Services](backup-azure-vms-first-look-arm.md)
> * [Ochrona maszyn wirtualnych platformy Azure przy użyciu magazynu usługi Backup](backup-azure-vms-first-look.md)
>
>

Ten samouczek przedstawia kroki tworzenia kopii zapasowej maszyny wirtualnej platformy Azure w magazynie usługi Backup na platformie Azure. W tym artykule opisano klasyczny model wdrażania (model wdrażania Service Manager) na potrzeby tworzenia kopii zapasowych maszyn wirtualnych. Poniższa procedura dotyczy tylko magazynów kopii zapasowych tworzonych w klasycznym portalu. W przypadku nowych wdrożeń firma Microsoft zaleca używanie modelu opartego na programie Resource Manager.

Jeśli chcesz tworzyć kopie zapasowe maszyny wirtualnej w magazynie usługi Recovery Services należącym do grupy zasobów, zobacz [Pierwsze spojrzenie: ochrona maszyn wirtualnych przy użyciu magazynu usługi Recovery Services](backup-azure-vms-first-look-arm.md).

Do pomyślnego ukończenia tego samouczka muszą być spełnione następujące wymagania wstępne:

* Utworzona została maszyna wirtualna w ramach subskrypcji platformy Azure.
* Maszyna wirtualna ma łączność z publicznymi adresami IP platformy Azure. Aby uzyskać więcej informacji, zobacz [Network connectivity](backup-azure-vms-prepare.md#network-connectivity) (Łączność sieciowa).


> [!NOTE]
> Platforma Azure ma dwa modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [Resource Manager i model klasyczny](../azure-resource-manager/resource-manager-deployment-model.md). Ten samouczek jest przeznaczony dla maszyn wirtualnych utworzonych w klasycznym portalu.
>
>

## <a name="create-a-backup-vault"></a>Tworzenie magazynu kopii zapasowych
Magazyn kopii zapasowych to jednostka, w której przechowywane są wszystkie kopie zapasowe i punkty odzyskiwania, które zostały utworzone na przestrzeni czasu. Magazyn kopii zapasowych zawiera również zasady tworzenia kopii zapasowych stosowane względem maszyn wirtualnych, których kopie zapasowe są tworzone.

> [!IMPORTANT]
> Począwszy od marca 2017 r. nie można już tworzyć magazynów kopii zapasowych za pomocą klasycznego portalu. Istniejące magazyny kopii zapasowych są nadal obsługiwane i możliwe jest [użycie środowiska Azure PowerShell w celu utworzenia magazynów kopii zapasowych](./backup-client-automation-classic.md#create-a-backup-vault). Firma Microsoft zaleca jednak tworzenie dla wszystkich wdrożeń magazynów usługi Recovery Services, ponieważ przyszłe rozszerzenia będą miały zastosowanie tylko do magazynów tej usługi.



## <a name="discover-and-register-azure-virtual-machines"></a>Odnajdywanie i rejestrowanie maszyn wirtualnych platformy Azure
Przed zarejestrowaniem maszyny wirtualnej w magazynie uruchom proces wykrywania, aby zidentyfikować wszelkie nowe maszyny wirtualne. Proces ten zwraca listę maszyn wirtualnych w ramach subskrypcji wraz z dodatkowymi informacjami, takimi jak nazwa usługi w chmurze i region.

1. Zaloguj się do [klasycznego portalu Azure](http://manage.windowsazure.com/).
2. W klasycznym portalu Azure kliknij pozycję **Usługi odzyskiwania**, aby otworzyć listę magazynów Usług odzyskiwania.
    ![Wybór obciążenia](./media/backup-azure-vms-first-look/recovery-services-icon.png)
3. Z listy magazynów wybierz magazyn, w którym zostanie utworzona kopia zapasowa maszyny wirtualnej.

    Po wybraniu magazyn zostanie otwarty na stronie **Szybki start**.
4. W menu magazynu kliknij pozycję **Zarejestrowane elementy**.

    ![Wybieranie obciążenia](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. Z menu **Typ** wybierz polecenie **Maszyna wirtualna platformy Azure**.

    ![Wybór obciążenia](./media/backup-azure-vms/discovery-select-workload.png)
6. Kliknij pozycję **ODNAJDŹ** w dolnej części strony.
    ![Przycisk Odnajdź](./media/backup-azure-vms/discover-button-only.png)

    Proces odnajdywania może zająć kilka minut, gdy maszyny wirtualne są wyszczególniane. W dolnej części ekranu znajduje się powiadomienie informujące, że proces jest uruchomiony.

    ![Odnajdywanie maszyn wirtualnych](./media/backup-azure-vms/discovering-vms.png)

    Powiadomienie ulega zmianie, gdy proces zostanie zakończony.

    ![Odnajdywanie zostało zakończone](./media/backup-azure-vms-first-look/discovery-complete.png)
7. Kliknij pozycję **ZAREJESTRUJ** w dolnej części strony.
    ![Przycisk Zarejestruj](./media/backup-azure-vms-first-look/register-icon.png)
8. W menu skrótów **Zarejestruj elementy** wybierz maszyny wirtualne, które chcesz zarejestrować.

   > [!TIP]
   > W tym samym czasie można zarejestrować wiele maszyn wirtualnych.
   >
   >

    Zadanie jest tworzone dla każdej maszyny wirtualnej, która zostanie wybrana.
9. W powiadomieniu kliknij pozycję **Wyświetl zadanie**, aby przejść do strony **Zadania**.

    ![Rejestrowanie zadania](./media/backup-azure-vms/register-create-job.png)

    Maszyna wirtualna pojawia się również na liście zarejestrowanych elementów wraz ze stanem operacji rejestrowania.

    ![Rejestrowanie — stan 1](./media/backup-azure-vms/register-status01.png)

    Po zakończeniu operacji stan zostanie zmieniony w celu odzwierciedlenia stanu oznaczającego *zarejestrowanie*.

    ![Rejestrowanie — stan 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Instalowanie agenta maszyny wirtualnej na maszynie wirtualnej
Aby rozszerzenie usługi Backup mogło działać, na maszynie wirtualnej Azure musi być zainstalowany agent maszyny wirtualnej. Jeśli maszyna wirtualna została utworzona z poziomu galerii Azure, agent maszyny wirtualnej znajduje się już na maszynie wirtualnej. Możesz przejść do sekcji związanej z [ochroną maszyn wirtualnych](backup-azure-vms-first-look.md#create-the-backup-policy).

Jeśli Twoja maszyna wirtualna została zmigrowana z lokalnego centrum danych, to prawdopodobnie nie ma na niej zainstalowanego agenta maszyny wirtualnej. Konieczne jest zainstalowanie agenta maszyny wirtualnej na maszynie wirtualnej zanim możliwe będzie chronienie maszyny wirtualnej. Aby uzyskać szczegółowy opis kroków instalowania agenta maszyny wirtualnej, zobacz [sekcję VM Agent (Agent maszyny wirtualnej) w artykule dotyczącym tworzenia kopii zapasowych maszyn wirtualnych](backup-azure-vms-prepare.md#vm-agent).

## <a name="create-the-backup-policy"></a>Tworzenie zasad kopii zapasowych
Przed wyzwoleniem zadania tworzenia początkowej kopii zapasowej ustaw harmonogram tworzenia migawek kopii zapasowych. Zasady tworzenia kopii zapasowej określają harmonogram tworzenia migawek kopii zapasowych oraz okres, przez który te migawki są przechowywane. Informacje dotyczące przechowywania są oparte na schemacie rotacji kopii zapasowej dziadek-ojciec-syn.

1. Przejdź do magazynu kopii zapasowych w obszarze **Usługi odzyskiwania** klasycznego portalu Azure, a następnie kliknij pozycję **Zarejestrowane elementy**.
2. Z menu rozwijanego wybierz pozycję **Maszyna wirtualna platformy Azure**.

    ![Wybieranie obciążenia w portalu](./media/backup-azure-vms/select-workload.png)
3. Kliknij pozycję **CHROŃ** w dolnej części strony.
    ![Klikanie pozycji Chroń](./media/backup-azure-vms-first-look/protect-icon.png)

    Zostanie wyświetlony **kreator ochrony elementów** i zostanie wyświetlona lista *tylko* tych maszyn wirtualnych, które są zarejestrowane i nie są chronione.

    ![Konfigurowanie ochrony w dużej skali](./media/backup-azure-vms/protect-at-scale.png)
4. Wybierz maszyny wirtualne, które chcesz chronić.

    Jeśli istnieje więcej niż jedna maszyna wirtualna o tej samej nazwie, użyj usługi Cloud Services w celu rozróżnienia maszyn wirtualnych.
5. W menu **Konfigurowanie ochrony** wybierz istniejące zasady lub utwórz nowe zasady w celu ochrony zidentyfikowanych maszyn wirtualnych.

    Z nowymi magazynami kopii zapasowych są skojarzone zasady domyślne. Te zasady określają tworzenie każdego wieczoru codziennych migawek, które są przechowywane przez 30 dni. Z jednymi zasadami tworzenia kopii zapasowych może być skojarzonych wiele maszyn wirtualnych. Maszyna wirtualna może być jednak jednocześnie skojarzona tylko z jednymi zasadami.

    ![Ochrona za pomocą nowych zasad](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Zasady tworzenia kopii zapasowych obejmują schemat przechowywania dla zaplanowanych kopii zapasowych. W przypadku wybrania istniejących zasad tworzenia kopii zapasowych w następnym kroku nie będzie możliwe zmodyfikowanie opcji przechowywania.
   >
   >
6. W polu **Zakres przechowywania** zdefiniuj codzienny, cotygodniowy, comiesięczny i coroczny zakres dla określonych punktów kopii zapasowych.

    ![Kopia zapasowa maszyny wirtualnej jest tworzona z punktem odzyskiwania](./media/backup-azure-vms/long-term-retention.png)

    Zasady przechowywania określają czas przechowywania kopii zapasowej. Możliwe jest określenie różnych zasad przechowywania na podstawie czasu tworzenia kopii zapasowej.
7. Kliknij pozycję **Zadania**, aby wyświetlić listę zadań **Konfiguracja ochrony**.

    ![Zadanie konfigurowania ochrony](./media/backup-azure-vms/protect-configureprotection.png)

    Po ustanowieniu zasad przejdź do następnego kroku i uruchom tworzenie początkowej kopii zapasowej.

## <a name="initial-backup"></a>Początkowa kopia zapasowa
Gdy zostanie ustawiona ochrona maszyny wirtualnej za pomocą zasad, można wyświetlić tę relację na karcie **Chronione elementy**. Dopóki nie zostanie utworzona początkowa kopia zapasowa, pole **Stan ochrony** będzie miało wartość **Chroniona — (oczekiwanie na początkową kopię zapasową)**. Domyślnie pierwszą zaplanowaną kopią zapasową jest *początkowa kopia zapasowa*.

![Oczekiwanie na kopię zapasową](./media/backup-azure-vms-first-look/protection-pending-border.png)

Aby uruchomić proces tworzenia początkowej kopii zapasowej natychmiast:

1. Na stronie **Chronione elementy** kliknij pozycję **Utwórz kopię zapasową teraz** w dolnej części strony.
    ![Ikona Utwórz kopię zapasową teraz](./media/backup-azure-vms-first-look/backup-now-icon.png)

    Usługa Azure Backup tworzy zadanie dla operacji tworzenia początkowej kopii zapasowej.
2. Kliknij kartę **Zadania**, aby wyświetlić listę zadań.

    ![Tworzenie kopii zapasowej w toku](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Po zakończeniu tworzenia początkowej kopii zapasowej stan maszyny wirtualnej na karcie **Chronione elementy** ma wartość *Chroniona*.

    ![Kopia zapasowa maszyny wirtualnej jest tworzona z punktem odzyskiwania](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > Tworzenie kopii zapasowych maszyn wirtualnych jest procesem lokalnym. Nie można tworzyć kopii zapasowych maszyn wirtualnych z jednego regionu w magazynie kopii zapasowych znajdującym się w innym regionie. W związku z tym w każdym regionie Azure, dla którego mają zostać utworzone kopie zapasowe maszyn wirtualnych, należy utworzyć co najmniej jeden magazyn kopii zapasowych.
   >
   >

## <a name="next-steps"></a>Następne kroki
Po pomyślnym utworzeniu kopii zapasowej maszyny wirtualnej można wykonać jeden z kilku kroków. Najbardziej logicznym krokiem jest zapoznanie się z przywracaniem danych do maszyny wirtualnej. Istnieją jednak również zadania zarządzania, które ułatwią zrozumienie sposobu zapewniania bezpieczeństwa danych i ograniczania kosztów.

* [Monitorowanie maszyn wirtualnych i zarządzanie nimi](backup-azure-manage-vms.md)
* [Przywracanie maszyn wirtualnych](backup-azure-restore-vms.md)
* [Wskazówki dotyczące rozwiązywania problemów](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a>Pytania?
Jeśli masz pytania lub jeśli brakuje Ci jakiejś funkcji, [prześlij nam opinię](http://aka.ms/azurebackup_feedback).

