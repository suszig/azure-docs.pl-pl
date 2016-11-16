---
title: "Pierwsze spojrzenie: ochrona maszyn wirtualnych platformy Azure przy użyciu magazynu usługi Backup | Microsoft Docs"
description: "Ochrona maszyn wirtualnych platformy Azure przy użyciu magazynu usługi Backup. Ten samouczek wyjaśnia sposób tworzenia magazynu, rejestrowania maszyn wirtualnych, tworzenia zasad i ochrony maszyn wirtualnych na platformie Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/15/2016
ms.author: markgal; jimpark
translationtype: Human Translation
ms.sourcegitcommit: 9eddde0df61ac7dde9b24f973e7eec786cd48f24
ms.openlocfilehash: b8f62d1fab4b9132f35e2fab632968cf001c832d


---
# <a name="first-look-backing-up-azure-virtual-machines"></a>Pierwsze spojrzenie: tworzenie kopii zapasowych maszyn wirtualnych platformy Azure
> [!div class="op_single_selector"]
> * [Ochrona maszyn wirtualnych przy użyciu magazynu usługi Recovery Services](backup-azure-vms-first-look-arm.md)
> * [Ochrona maszyn wirtualnych platformy Azure przy użyciu magazynu usługi Backup](backup-azure-vms-first-look.md)
>
>

Ten samouczek przedstawia kroki tworzenia kopii zapasowej maszyny wirtualnej platformy Azure w magazynie usługi Backup na platformie Azure. W tym artykule opisano klasyczny model wdrażania (model wdrażania Service Manager) na potrzeby tworzenia kopii zapasowych maszyn wirtualnych. Jeśli chcesz tworzyć kopie zapasowe maszyny wirtualnej w magazynie usługi Recovery Services należącym do grupy zasobów, zobacz [Pierwsze spojrzenie: ochrona maszyn wirtualnych przy użyciu magazynu usługi Recovery Services](backup-azure-vms-first-look-arm.md). Do pomyślnego ukończenia tego samouczka muszą być spełnione następujące wymagania wstępne:

* Utworzona została maszyna wirtualna w ramach subskrypcji platformy Azure.
* Maszyna wirtualna ma łączność z publicznymi adresami IP platformy Azure. Aby uzyskać więcej informacji, zobacz [Network connectivity](backup-azure-vms-prepare.md#network-connectivity) (Łączność sieciowa).

Aby utworzyć kopię zapasową maszyny wirtualnej, należy wykonać pięć głównych kroków:  

![step-one](./media/backup-azure-vms-first-look/step-one.png) Utworzenie magazynu kopii zapasowych lub wskazanie istniejącego magazynu kopii zapasowych. <br/>
![step-two](./media/backup-azure-vms-first-look/step-two.png) Użycie klasycznej witryny Azure Portal do odnalezienia i zarejestrowania maszyn wirtualnych. <br/>
![step-three](./media/backup-azure-vms-first-look/step-three.png) Zainstalowanie agenta maszyny wirtualnej. <br/>
![step-four](./media/backup-azure-vms-first-look/step-four.png) Utworzenie zasad ochrony maszyn wirtualnych. <br/>
![step-five](./media/backup-azure-vms-first-look/step-five.png) Uruchomienie procesu tworzenia kopii zapasowej.

![Ogólny widok procesu tworzenia kopii zapasowej maszyny wirtualnej](./media/backup-azure-vms-first-look/backupazurevm-classic.png)

> [!NOTE]
> Platforma Azure ma dwa modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [Resource Manager i model klasyczny](../resource-manager-deployment-model.md). Ten samouczek jest przeznaczony dla maszyn wirtualnych, które mogą zostać utworzone w klasycznym portalu Azure. Usługa Azure Backup obsługuje maszyny wirtualne oparte na usłudze Resource Manager. Aby uzyskać szczegółowe informacje na temat tworzenia kopii zapasowych maszyn wirtualnych w magazynie usługi Recovery Services, zobacz [Pierwsze spojrzenie: ochrona maszyn wirtualnych przy użyciu magazynu usługi Recovery Services](backup-azure-vms-first-look-arm.md).
>
>

## <a name="step-1-create-a-backup-vault-for-a-vm"></a>Krok 1. Tworzenie magazynu kopii zapasowych dla maszyny wirtualnej
Magazyn kopii zapasowych to jednostka, w której przechowywane są wszystkie kopie zapasowe i punkty odzyskiwania, które zostały utworzone na przestrzeni czasu. Magazyn kopii zapasowych zawiera również zasady tworzenia kopii zapasowych stosowane względem maszyn wirtualnych, których kopie zapasowe są tworzone.

1. Zaloguj się do [klasycznego portalu Azure](http://manage.windowsazure.com/).
2. W lewym dolnym rogu Portalu Azure kliknij pozycję **Nowy**.

    ![Klikanie menu Nowy](./media/backup-azure-vms-first-look/new-button.png)
3. W kreatorze Szybkie tworzenie kliknij pozycję **Usługi danych** > **Usługi odzyskiwania** > **Magazyn kopii zapasowych** > **Szybkie tworzenie**.

    ![Tworzenie magazynu kopii zapasowych](./media/backup-azure-vms-first-look/new-vault-wizard-one-subscription.png)

    Kreator wyświetli monit o podanie wartości w polach **Nazwa** i **Region**. W przypadku administrowania więcej niż jedną subskrypcją zostanie wyświetlone okno dialogowe wyboru subskrypcji.
4. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Nazwa musi być unikalna w ramach subskrypcji platformy Azure.
5. W polu **Region** wybierz region geograficzny dla magazynu. Magazyn **musi** znajdować się w tym samym regionie, w którym znajdują się chronione przez niego maszyny wirtualne.

    Jeśli nie znasz regionu, w którym znajduje się Twoja maszyna wirtualna, zamknij kreatora, a następnie kliknij pozycję **Maszyny wirtualne** na liście usług Azure. Kolumna Lokalizacja zawiera nazwę regionu. Jeśli masz maszyny wirtualne w wielu regionach, utwórz magazyn kopii zapasowych w każdym regionie.
6. Jeśli w kreatorze nie zostanie wyświetlone okno dialogowe **Subskrypcja**, przejdź do następnego kroku. Jeśli pracujesz z wieloma subskrypcjami, wybierz subskrypcję do skojarzenia z nowym magazynem kopii zapasowych.

    ![Powiadomienie wyskakujące tworzenia magazynu](./media/backup-azure-vms-first-look/backup-vaultcreate.png)
7. Kliknij pozycję **Utwórz magazyn**. Tworzenie magazynu kopii zapasowych może zająć trochę czasu. Stan powiadomień można monitorować w dolnej części portalu.

    ![Powiadomienie wyskakujące tworzenia magazynu](./media/backup-azure-vms-first-look/create-vault-demo.png)

    Po pomyślnym utworzeniu magazynu zostanie wyświetlony komunikat potwierdzający. Magazyn jest wyświetlany na stronie **Usługi odzyskiwania** jako **Aktywny**.

    ![Powiadomienie wyskakujące tworzenia magazynu](./media/backup-azure-vms-first-look/create-vault-demo-success.png)
8. Na liście magazynów na stronie **Usługi odzyskiwania** wybierz utworzony magazyn, aby otworzyć stronę **Szybki start**.

    ![Lista magazynów kopii zapasowych](./media/backup-azure-vms-first-look/active-vault-demo.png)
9. Na stronie **Szybki start** kliknij pozycję **Konfiguruj**, aby otworzyć obszar opcji replikacji magazynu.
    ![Lista magazynów kopii zapasowych](./media/backup-azure-vms-first-look/configure-storage.png)
10. W obszarze opcji **replikacji magazynu** wybierz opcję replikacji dla magazynu.

    ![Lista magazynów kopii zapasowych](./media/backup-azure-vms-first-look/backup-vault-storage-options-border.png)

    Domyślnie magazyn jest nadmiarowy geograficznie. Wybierz magazyn geograficznie nadmiarowy, jeśli jest to Twoja podstawowa kopia zapasowa. Wybierz magazyn lokalnie nadmiarowy, jeśli chcesz skorzystać z tańszej, ale mniej trwałej opcji. Aby uzyskać więcej informacji o opcjach magazynu geograficznie nadmiarowego i lokalnie nadmiarowego, zobacz [Azure Storage replication overview](../storage/storage-redundancy.md) (Omówienie replikacji usługi Azure Storage).

Po wybraniu opcji magazynu dla Twojego magazynu możesz skojarzyć maszynę wirtualną z magazynem. Aby rozpocząć kojarzenie, odnajdź i zarejestruj maszyny wirtualne platformy Azure.

## <a name="step-2-discover-and-register-azure-virtual-machines"></a>Krok 2. Odnajdywanie i rejestrowanie maszyn wirtualnych platformy Azure
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

## <a name="step-3-install-the-vm-agent-on-the-virtual-machine"></a>Krok 3. Instalowanie agenta maszyny wirtualnej na maszynie wirtualnej
Agent maszyny wirtualnej musi być zainstalowany na maszynie wirtualnej platformy Azure, aby rozszerzenie usługi Backup mogło działać. Jeśli maszyna wirtualna została utworzona z poziomu galerii Azure, agent maszyny wirtualnej znajduje się już na maszynie wirtualnej. Możesz przejść do sekcji związanej z [ochroną maszyn wirtualnych](backup-azure-vms-first-look.md#step-4---create-the-backup-policy).

Jeśli Twoja maszyna wirtualna została zmigrowana z lokalnego centrum danych, to prawdopodobnie nie ma na niej zainstalowanego agenta maszyny wirtualnej. Konieczne jest zainstalowanie agenta maszyny wirtualnej na maszynie wirtualnej zanim możliwe będzie chronienie maszyny wirtualnej. Aby uzyskać szczegółowy opis kroków instalowania agenta maszyny wirtualnej, zobacz [sekcję VM Agent (Agent maszyny wirtualnej) w artykule dotyczącym tworzenia kopii zapasowych maszyn wirtualnych](backup-azure-vms-prepare.md#vm-agent).

## <a name="step-4-create-the-backup-policy"></a>Krok 4. Tworzenie zasad wykonywania kopii zapasowej
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

## <a name="step-5-initial-backup"></a>Krok 5. Początkowa kopia zapasowa
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



<!--HONumber=Nov16_HO2-->


