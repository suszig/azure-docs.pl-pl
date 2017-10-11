---
title: " Usuwanie magazynu usług odzyskiwania na platformie Azure | Dokumentacja firmy Microsoft "
description: "Jak usunąć magazyn usług odzyskiwania i kopia zapasowa Azure. Magazyn kopii zapasowych można wywołać chmury Azure magazyn lub magazyn Azure recovery. Rozwiązywanie problemów, gdy nie można usunąć magazynu kopii zapasowych w portalu Azure lub klasycznego portalu."
services: service-name
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 5fa08157-2612-4020-bd90-f9e3c3bc1806
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: ae4a73d12898c62fe2c5cf3683bc7c1c8c845fdf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="delete-a-recovery-services-vault"></a>Usuwanie magazynu usługi Recovery Services
Usługa Azure Backup ma dwa typy magazynów — magazyn kopii zapasowych i magazyn usługi Recovery Services. Najpierw powstał magazyn kopii zapasowych. Następnie wprowadzono magazyn usługi Recovery Services, aby zapewnić obsługę rozszerzonych wdrożeń usługi Resource Manager. Ze względu na rozszerzone możliwości i zależności informacje, które muszą być przechowywane w magazynie usuwanie magazynu usług odzyskiwania lub kopii zapasowej może być trudne. W tym artykule opisano sposób usuwania magazynów w klasycznym portalu i portalu Azure.  

| **Typ wdrożenia** | **Portal** | **Nazwa magazynu** |
| --- | --- | --- |
| Wdrożenie klasyczne |Wdrożenie klasyczne |Magazyn kopii zapasowych |
| Resource Manager |Azure |Magazyn usługi Recovery Services |

> [!NOTE]
> Magazyny kopii zapasowych nie mogą chronić rozwiązań wdrożonych przy użyciu usługi Resource Manager. Jednak można użyć magazynu usług odzyskiwania do ochrony w klasycznym wdrożonych serwerach i maszyn wirtualnych.  
>

> [!IMPORTANT]
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> **15 października 2017 r.**: nie będzie już można tworzyć magazynów kopii zapasowych przy użyciu programu PowerShell. <br/> **Od 1 listopada 2017 roku**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
>

W tym artykule używany jest termin, magazynu, aby odwołać się do formularz ogólny magazyn kopii zapasowych lub magazyn usług odzyskiwania. Używamy nazwy formalne, Magazyn kopii zapasowych lub magazyn usług odzyskiwania, gdy jest to niezbędne do rozróżniania między magazynami.

## <a name="deleting-a-recovery-services-vault"></a>Usuwanie magazynu usług Recovery Services
Usuwanie magazynu usług odzyskiwania jest jednoetapowy proces - *pod warunkiem magazynu nie zawiera żadnych zasobów*. Przed usunięciem magazynu usług odzyskiwania, należy usunąć lub usunąć wszystkie zasoby w magazynie. Jeśli próbujesz usunąć magazynu, który zawiera zasoby, występuje błąd, podobnie jak na poniższej ilustracji:

![Błąd usuwania magazynu](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Dopóki zasobów z magazynu zostały wyczyszczone, klikając pozycję **ponów** tworzy ten sam błąd. Jeśli użytkownik pozostaje zablokowany na ten komunikat o błędzie, kliknij przycisk **anulować** i wykonaj następujące kroki, aby usunąć zasoby w magazynie.

### <a name="removing-the-items-from-a-vault-protecting-a-vm"></a>Usuwanie elementów z magazynu ochrony maszyn wirtualnych
Jeśli masz już magazyn usług odzyskiwania, Otwórz, przejdź do kroku drugim.

1. Otwórz Azure portal, a z poziomu pulpitu nawigacyjnego magazynu, które chcesz usunąć.

   Jeśli nie masz magazyn usług odzyskiwania przypięty do pulpitu nawigacyjnego, w menu centralnym kliknij **więcej usług** i na liście zasobów wpisz **usług odzyskiwania**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Kliknij przycisk **Magazyny usług odzyskiwania**.

   ![Tworzenie magazynu usługi Recovery Services — krok 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Zostanie wyświetlona lista magazynów usług odzyskiwania. Z listy wybierz magazyn, który chcesz usunąć.

   ![Wybierz magazyn z listy](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. W widoku magazynu przyjrzeć się **Essentials** okienka. Aby usunąć magazynu, nie może być wszystkie chronione elementy. Jeśli jest wyświetlana liczba różne od zera, w obszarze albo **kopii zapasowej elementów** lub **kopii zapasowej serwerów zarządzania**, przed usunięciem magazynu należy usunąć te elementy.

    ![Szukaj w okienku Essentials elementy chronione](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    Maszyny wirtualne i pliki i foldery są traktowane jako elementy kopii zapasowej i znajduje się w **kopii zapasowej elementów** obszar okienka Essentials. Serwer DPM znajduje się w **kopii zapasowej serwera zarządzania** obszar okienka Essentials. **Elementy replikowane** odnoszą się do usługi Azure Site Recovery.
3. Aby rozpocząć, usuwanie chronione elementy z magazynu, Znajdź elementy w magazynie. Na pulpicie nawigacyjnym magazynu kliknij **ustawienia**, a następnie kliknij przycisk **kopii zapasowej elementów** do otwarcia tego bloku.

    ![Wybierz magazyn z listy](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    **Kopii zapasowej elementów** bloku ma osobnych list, na podstawie typu elementu: maszyny wirtualne platformy Azure lub folderów plików (zobacz obraz). Lista typu elementu domyślne, wyświetlany jest maszynach wirtualnych platformy Azure. Zaznacz, aby wyświetlić listę elementów folderów plików w magazynie **folderów plików** z menu rozwijanego.
4. Przed usunięciem elementu z magazynu ochrony maszyn wirtualnych, należy zatrzymać zadanie tworzenia kopii zapasowej elementów i usunięcie danych punktu odzyskiwania. Dla każdego elementu w magazynie wykonaj następujące kroki:

    a. Na **elementów kopii zapasowych** bloku, kliknij element prawym przyciskiem myszy i wybierz z menu kontekstowego **kopii zapasowej Zatrzymaj**.

    ![Zatrzymaj zadanie tworzenia kopii zapasowej](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    Zostanie otwarty blok zatrzymać kopii zapasowej.

    b. Na **zatrzymać kopii zapasowej** bloku z **wybierz opcję** menu, wybierz opcję **usunąć danych kopii zapasowej** > wpisz nazwę elementu > i kliknij przycisk **kopii zapasowej Zatrzymaj**.

    Wpisz nazwę elementu, aby sprawdzić, czy chcesz go usunąć. **Zatrzymać kopii zapasowej** aktywuje przycisk po zweryfikowaniu elementu. Jeśli nie ma okno dialogowe, aby wpisać nazwę elementu kopii zapasowej, **Zachowaj dane kopii zapasowej** opcji.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Opcjonalnie możesz podać przyczyny, dlaczego usuwasz danych i Dodaj komentarze. Po kliknięciu **zatrzymać kopii zapasowej**, Zezwalaj Usuń zadanie do wykonania przed próbą usunięcia w magazynie. Aby sprawdzić, czy zadanie zostało zakończone, sprawdź komunikaty Azure ![usunąć danych kopii zapasowej](./media/backup-azure-delete-vault/messages.png). <br/>
    Po zakończeniu zadania pojawi się komunikat z informacją o procesu tworzenia kopii zapasowej zostało zatrzymane i dane kopii zapasowej dla tego elementu, został usunięty.

    c. Po usunięciu elementu z listy na **kopii zapasowej elementów** menu, kliknij przycisk **Odśwież** pozostałych elementów w magazynie.

      ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/empty-items-list.png)

      Jeśli nie ma żadnych elementów na liście, przewiń do **Essentials** okienka w bloku magazynu kopii zapasowej. Nie powinny istnieć żadnego **kopii zapasowej elementów**, **kopii zapasowej serwerów zarządzania**, lub **elementy replikowane** na liście. Jeśli nadal wyświetlane w magazynie, wróć do kroku 3 i wybierz typ innego elementu listy.  
5. Jeśli nie ma więcej elementów na pasku narzędzi magazynu, kliknij przycisk **usunąć**.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/delete-vault.png)
6. Aby sprawdzić, czy chcesz usunąć magazyn, kliknij przycisk **tak**.

    Magazyn zostanie usunięty, a następnie przywraca portalu **nowy** menu usługi.

## <a name="what-if-i-stopped-the-backup-process-but-retained-the-data"></a>Co zrobić, jeśli zatrzymano proces tworzenia kopii zapasowej, ale przechowywane dane?
Jeśli jednak przypadkowo zatrzymano proces tworzenia kopii zapasowej *zachowane* danych, należy usunąć dane kopii zapasowej przed usunięciem magazynu. Aby usunąć dane kopii zapasowej:

1. Na **elementów kopii zapasowych** bloku, kliknij element prawym przyciskiem myszy, a następnie w menu kontekstowym kliknij **usunąć danych kopii zapasowej**.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    **Usunąć dane z kopii zapasowej** zostanie otwarty blok.
2. Na **usunąć dane z kopii zapasowej** bloku, wpisz nazwę elementu, a następnie kliknij przycisk **usunąć**.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Po usunięciu danych, wróć do kroku 4c i kontynuować proces.

## <a name="delete-a-vault-used-to-protect-a-dpm-server"></a>Usuwanie magazynu używane do ochrony serwera programu DPM
Przed usunięciem magazynu używane do ochrony serwera programu DPM, należy wyczyścić punktów odzyskiwania, które zostały utworzone i następnie Wyrejestruj serwer z magazynu.

Aby usunąć dane skojarzone z grupy ochrony:

1. W konsoli administratora programu DPM, kliknij przycisk **ochrony** > Wybierz grupę ochrony > Wybierz członka grupy ochrony > i na wstążce narzędzi kliknij **Usuń**.

  Wybierz członka grupy ochrony, aby aktywować **Usuń** przycisku na wstążce narzędzi. W tym przykładzie element członkowski jest **dummyvm9**. Aby wybrać wiele elementów członkowskich grupy ochrony, przytrzymaj klawisz Ctrl, klikając elementy członkowskie.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    **Zatrzymaj ochronę** zostanie otwarte okno dialogowe.
2. W **Zatrzymaj ochronę** okno dialogowe, wybierz opcję **Usuń chronione dane**i kliknij przycisk **Zatrzymaj ochronę**.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    Można usunąć magazynu, należy wyczyścić, lub usunąć magazyn chronionych danych. W zależności od liczby punktów odzyskiwania i danych w grupie ochrony może upłynąć dowolnym od kilku sekund do kilku minut do usunięcia danych. **Zatrzymaj ochronę** okna dialogowego pokazuje stan po ukończeniu zadania.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. Kontynuować ten proces dla wszystkich członków we wszystkich grupach ochrony.

    Usuń wszystkie chronione dane i ochronę grupy.
4. Po usunięciu wszystkich członków z grupy ochrony, przełącz się do portalu Azure. Otwórz pulpit nawigacyjny magazynu i upewnij się, że istnieją nie **kopii zapasowej elementów**, **kopii zapasowej serwerów zarządzania**, lub **elementy replikowane**. Kliknij na pasku narzędzi magazynu **usunąć**.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/delete-vault.png)

    W przypadku serwerów zarządzania kopiami zapasowymi w zarejestrowany w magazynie, nie można usunąć magazynu, nawet jeśli nie ma żadnych danych w magazynie. Jeśli usunięto serwerów zarządzania kopiami zapasowymi skojarzonego z magazynem, ale istnieją serwery wymienione w **Essentials** okienku, zobacz [znaleźć serwerów zarządzania kopiami zapasowymi zarejestrowany w magazynie](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault).
5. Aby sprawdzić, czy chcesz usunąć magazyn, kliknij przycisk **tak**.

    Magazyn zostanie usunięty, a następnie przywraca portalu **nowy** menu usługi.

## <a name="delete-a-vault-used-to-protect-a-production-server"></a>Usuwanie magazynu używane do ochrony serwera produkcyjnego
Przed usunięciem magazynu używane do ochrony serwera produkcyjnego, należy usunąć lub Wyrejestruj serwer z magazynu.

Aby usunąć serwer produkcyjny skojarzonego z magazynem:

1. W portalu Azure, otwarcia pulpitu nawigacyjnego magazynu, a następnie kliknij przycisk **ustawienia** > **infrastruktura kopii zapasowej** > **serwerów produkcyjnych**.

    ![Otwiera blok serwerów produkcyjnych.](./media/backup-azure-delete-vault/delete-production-server.png)

    **Serwerów produkcyjnych** bloku otwiera i wyświetla listę wszystkich serwerów produkcyjnych w magazynie.

    ![Lista serwerów produkcyjnych.](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. Na **serwerów produkcyjnych** bloku, kliknij prawym przyciskiem myszy na serwerze, a następnie kliknij przycisk **usunąć**.

    ![Usuwanie serwera produkcyjnego ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    **Usunąć** zostanie otwarty blok.

    ![Usuwanie serwera produkcyjnego ](./media/backup-azure-delete-vault/delete-blade.png)
3. Na **usunąć** bloku, sprawdź nazwę serwera i kliknij **usunąć**. Należy poprawnie nazwy serwera, aby aktywować **usunąć** przycisku.

    Po usunięciu magazynu, pojawi się komunikat informujący, że magazyn został usunięty. Po usunięciu wszystkich serwerów w magazynie, przewiń do okienka Essentials na pulpicie nawigacyjnym magazynu.
4. Na pulpicie nawigacyjnym magazynu, upewnij się, brak nie **kopii zapasowej elementów**, **kopii zapasowej serwerów zarządzania**, lub **elementy replikowane**. Kliknij na pasku narzędzi magazynu **usunąć**.
5. Aby sprawdzić, czy chcesz usunąć magazyn, kliknij przycisk **tak**.

    Magazyn zostanie usunięty, a następnie przywraca portalu **nowy** menu usługi.

## <a name="delete-a-backup-vault-in-classic-portal"></a>Usuń magazyn kopii zapasowych w portalu klasycznym
Poniższe instrukcje dotyczą usuwanie magazynu kopii zapasowych w portalu klasycznym. Przed usunięciem magazynu kopii zapasowych, należy usunąć punktów odzyskiwania lub elementy kopię zapasową i Usuń zarejestrowane serwery. Zarejestrowane serwery są systemu Windows Server, stacji roboczej lub maszyn wirtualnych, które zostały zarejestrowane w magazynie.

1. Otwórz [klasyczny portal](https://manage.windowsazure.com).

2. Z listy magazynów kopii zapasowych wybierz magazyn, który chcesz usunąć.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/classic-portal-delete-vault-open-vault.png)

    Zostanie otwarty na pulpicie nawigacyjnym magazynu. Sprawdź liczbę serwerów z systemem Windows i/lub Azure maszyny wirtualne skojarzonego z magazynem. Ponadto sprawdź Całkowita ilość miejsca, które są używane na platformie Azure. Zatrzymaj wszystkie zadania tworzenia kopii zapasowej i usunięcie wszystkich danych przed usunięciem magazynu.

3. Kliknij przycisk **chronione elementy** , a następnie kliknij pozycję **Zatrzymaj ochronę**

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/classic-portal-delete-vault-stop-protect.png)

    **Zatrzymaj ochronę Twojego magazynu** zostanie wyświetlone okno dialogowe.
4. W **Zatrzymaj ochronę Twojego magazynu** dialogowym wyboru **Usuń skojarzone dane kopii zapasowej** i kliknij przycisk ![znacznikiem wyboru](./media/backup-azure-delete-vault/checkmark.png). <br/>
    Można opcjonalnie wybierz przyczynę zatrzymywanie ochrony i podaj komentarz.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/classic-portal-delete-vault-verify-stop-protect.png)

    Po usunięciu elementów w magazynie, Magazyn jest pusta.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/classic-portal-delete-vault-post-delete-data.png)
5. Na liście kart kliknij **zarejestrowane elementy**. **Typu** listy rozwijanej menu umożliwia wybranie typu serwera w zarejestrowany w magazynie. Typ może być serwerem systemu Windows lub maszynie wirtualnej platformy Azure. W poniższym przykładzie, wybierz maszynę wirtualną w zarejestrowany w magazynie, a następnie kliknij przycisk **Unregister**.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/classic-portal-unregister.png)

  Jeśli chcesz usunąć rejestrację w systemie Windows Server z **typu** menu rozwijanego wybierz **systemu Windows Server**, kliknij przycisk ![znacznikiem wyboru](./media/backup-azure-delete-vault/checkmark.png) odświeżyć ekran, a następnie kliknij przycisk **usunąć**. <br/>

  ![Wybierz serwer z systemem Windows](./media/backup-azure-delete-vault/select-windows-server.png)

6. Na liście kart kliknij **pulpitu nawigacyjnego** otworzyć danej karty. Sprawdź, czy nie ma żadnych zarejestrowanych serwerów lub chroniona w chmurze maszyn wirtualnych platformy Azure. Sprawdź także, że nie ma w magazynie danych. Kliknij przycisk **usunąć** usunąć magazyn.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/classic-portal-list-of-tabs-dashboard.png)

    Zostanie otwarty ekran potwierdzenia usunięcia kopii zapasowej magazynu. Wybierz opcję dlaczego jest usunięcie magazynu i kliknij przycisk ![Znacznik wyboru](./media/backup-azure-delete-vault/checkmark.png). <br/>

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-delete-vault/classic-portal-delete-vault-confirmation-1.png)

    Magazyn zostanie usunięty, a następnie zostanie ponownie wyświetlony pulpit nawigacyjny klasycznego portalu.

### <a name="find-the-backup-management-servers-registered-to-the-vault"></a>Znajdowanie serwerów zarządzania kopii zapasowej, w zarejestrowany w magazynie
Jeśli masz wiele serwerów zarejestrowanych w magazynie, może być trudne do zapamiętania je. Aby wyświetlić serwery zarejestrowane w magazynie i usunąć je:

1. Otwórz pulpit nawigacyjny magazynu.
2. W **Essentials** okienku, kliknij przycisk **ustawienia** do otwarcia tego bloku.

    ![Otwórz blok ustawień](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. Na **bloku ustawienia**, kliknij przycisk **infrastruktura kopii zapasowej**.
4. Na **infrastruktura kopii zapasowej** bloku, kliknij przycisk **serwerów zarządzania kopiami zapasowymi**. Zostanie otwarty blok serwerów zarządzania kopiami zapasowymi.

    ![Lista serwerów zarządzania kopiami zapasowymi](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. Aby usunąć serwer z listy, kliknij prawym przyciskiem myszy nazwę serwera, a następnie kliknij przycisk **usunąć**.
    **Usunąć** zostanie otwarty blok.
6. Na **usunąć** bloku, podaj nazwę serwera. Jeśli jest to nazwa długo, można skopiować i wkleić go z listy serwerów zarządzania kopiami zapasowymi. Następnie kliknij przycisk **usunąć**.  
