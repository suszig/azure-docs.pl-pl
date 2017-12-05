---
title: "Wykonywanie kopii zapasowych maszyn wirtualnych platformy Azure na dużą skalę | Dokumentacja firmy Microsoft"
description: "Równocześnie tworzyć kopie zapasowe wielu maszyn wirtualnych Azure"
services: backup
keywords: "kopii zapasowej maszyny wirtualnej. maszyny wirtualnej, wykonaj kopię zapasową; Tworzenie kopii zapasowej maszyny wirtualnej; Maszyna wirtualna kopii zapasowej; Kopia zapasowa Azure vm; Kopia zapasowa i odzyskiwanie po awarii"
author: markgalioto
ms.author: markgal
ms.date: 09/16/2017
ms.topic: tutorial
ms.service: backup
ms.custom: mvc
ms.openlocfilehash: 74ccf95b559b690eb53c2f4df14513dab5a94677
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="use-azure-portal-to-back-up-multiple-virtual-machines"></a>Aby utworzyć kopię zapasową wielu maszyn wirtualnych za pomocą portalu Azure

Podczas wykonywania kopii zapasowej danych na platformie Azure, dane są przechowywane w zasobów platformy Azure wywołuje magazyn usług odzyskiwania. Zasobu magazynu usług odzyskiwania jest dostępny w menu Ustawienia usług najbardziej Azure. O konieczności magazyn usług odzyskiwania zintegrowane w menu Ustawienia usług najbardziej Azure ułatwia bardzo kopii zapasowej danych. Jednak pojedynczo stosowaniu każdej bazy danych lub maszyny wirtualnej w firmie jest niewygodny. Co zrobić, jeśli chcesz utworzyć kopię zapasową danych dla wszystkich maszyn wirtualnych w jednym dziale lub w jednym miejscu? To proste utworzyć kopię zapasową wielu maszyn wirtualnych, tworząc zasady tworzenia kopii zapasowej i zastosowanie tej zasady do żądanego maszyn wirtualnych. Ten samouczek wyjaśnia, jak:

> [!div class="checklist"]
> * Tworzenie magazynu Usług odzyskiwania
> * Definiowanie zasad tworzenia kopii zapasowej
> * Zastosuj zasady tworzenia kopii zapasowej, aby chronić wiele maszyn wirtualnych
> * Wyzwalacz zadania tworzenia kopii zapasowej na żądanie dla chronionych maszyn wirtualnych

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

Magazyn usług odzyskiwania zawiera dane kopii zapasowej, a zasady tworzenia kopii zapasowej stosowane do chronionych maszyn wirtualnych. Tworzenie kopii zapasowych maszyn wirtualnych jest procesem lokalnym. Nie można utworzyć kopię maszynę wirtualną z jednej lokalizacji do magazynu usług odzyskiwania w innej lokalizacji. Tak dla każdej lokalizacji platformy Azure, że maszyny wirtualne do wykonania kopii zapasowej, co najmniej jeden magazyn usług odzyskiwania musi istnieć w tej lokalizacji.

1. W menu po lewej stronie wybierz **więcej usług** i na liście usług wpisz *usług odzyskiwania*. Podczas pisania odbywa się filtrowanie listy zasobów. Gdy pojawi się magazyny usług odzyskiwania na liście, wybierz go, aby otworzyć menu Magazyny usług odzyskiwania.

    ![Otwórz menu magazynu usług odzyskiwania](./media/tutorial-backup-vm-at-scale/full-browser-open-rs-vault.png) <br/>

2. W **Magazyny usług odzyskiwania** menu, kliknij przycisk **Dodaj** aby otworzyć menu magazynu usług odzyskiwania.

    ![Otwórz magazyn menu](./media/tutorial-backup-vm-at-scale/provide-vault-detail-2.png)

3. W usługach odzyskiwania magazynu menu 

    - Typ *myRecoveryServicesVault* w **nazwa**,
    - Bieżącej subskrypcji identyfikator jest wyświetlany w **subskrypcji**. Jeśli masz subskrypcje dodatkowe, można wybrać inną subskrypcję dla nowego magazynu.
    - Aby uzyskać **grupy zasobów** wybierz **Użyj istniejącego** i wybierz polecenie *myResourceGroup*. Jeśli *myResourceGroup* nie istnieje, wybierz **Utwórz nowy** i typ *myResourceGroup*.
    - Z **lokalizacji** menu rozwijanego wybierz *Europa*.
    - Kliknij przycisk **Utwórz** utworzyć magazyn usług odzyskiwania.

Magazyn usług odzyskiwania musi być w tej samej lokalizacji co maszyny wirtualne chronione. Jeśli masz maszyny wirtualne w wielu regionach, utwórz magazyn usług odzyskiwania w każdym regionie. W tym samouczku tworzy magazynu usług odzyskiwania w *Europa* ponieważ jest to miejsce *myVM* (maszyny wirtualnej utworzonej z szybkiego startu) został utworzony.

Utworzenie magazynu usług Recovery Services może potrwać kilka minut. Monitoruj powiadomienia o stanie wyświetlane w prawej górnej części obszaru portalu. Po utworzeniu magazynu pojawi się on na liście magazynów Usług odzyskiwania.

Podczas tworzenia magazynu usług odzyskiwania magazynu ma domyślnie magazyn geograficznie nadmiarowy. Aby zapewnić odporność danych, magazynu geograficznie nadmiarowego replikuje dane wielokrotnie dwóch regionach platformy Azure.

## <a name="set-backup-policy-to-protect-vms"></a>Ustaw zasady tworzenia kopii zapasowej, aby chronić maszyny wirtualne

Po utworzeniu magazynu usług odzyskiwania, następnym krokiem jest skonfigurowanie magazynu dla typu danych i można ustawić zasad tworzenia kopii zapasowej. Zasady tworzenia kopii zapasowej jest harmonogram kiedy i jak często są pobierane punktów odzyskiwania. Zasady zawierają także zakres przechowywania dla punktów odzyskiwania. W tym samouczku Załóżmy, że Twoja firma jest złożony z hoteli, stadium i restauracji i koncesji sportowych i ochrony danych na maszynach wirtualnych. Poniższe kroki tworzenia zasad tworzenia kopii zapasowej dla danych finansowych.

1. Wybierz z listy magazynów usług odzyskiwania **myRecoveryServicesVault** aby otworzyć jego pulpitu nawigacyjnego.

   ![Otwórz menu scenariusza](./media/tutorial-backup-vm-at-scale/open-vault-from-list.png)

2. W menu nawigacyjnym magazynu, kliknij polecenie **kopii zapasowej** aby otworzyć menu kopii zapasowej.

3. W menu Cel kopii zapasowej w **gdzie działa Twoje obciążenie** menu rozwijanego wybierz *Azure*. Z **co chcesz utworzyć kopię zapasową** listy rozwijanej, wybierz *maszyny wirtualnej*i kliknij przycisk **kopii zapasowej**.

    Te akcje Przygotuj magazyn usług odzyskiwania do interakcji z maszyną wirtualną. Magazyny usług odzyskiwania ma domyślne zasady, tworzy punkt przywracania, każdego dnia, którą przechowuje punkty przywracania na 30 dni.

    ![Otwórz menu scenariusza](./media/tutorial-backup-vm-at-scale/backup-goal.png)

4. Aby utworzyć nowe zasady, w menu zasad tworzenia kopii zapasowej z **wybierz zasady tworzenia kopii zapasowej** menu rozwijanego wybierz *Utwórz nowy*.

    ![Wybieranie obciążenia](./media/tutorial-backup-vm-at-scale/create-new-policy.png)

5. W **kopii zapasowej zasad** menu dla **Nazwa zasady** typu *Finance*. Wprowadź następujące zmiany zasad tworzenia kopii zapasowej: 
    - Aby uzyskać **częstotliwość wykonywania kopii zapasowych** Ustaw strefę czasową dla *czas środkowy*. Ponieważ złożone sportowych jest w stanie Teksas, właściciel chce chronometrażu lokalną. Pozostaw częstotliwość wykonywania kopii zapasowych ustawioną codziennie o 3:30 AM.
    - Aby uzyskać **przechowywanie codziennego punktu kopii zapasowej**, ustawienie okresu 90 dni.
    - Dla **przechowywanie tygodniowego punktu kopii zapasowej**, użyj *poniedziałek* punkt przywracania i przechowywać je 52 tygodni.
    - Aby uzyskać **przechowywanie miesięcznego punktu kopii zapasowej**, użyj punkt przywracania, od niedzieli pierwszego dnia miesiąca i przechowywać je do 36 miesięcy.
    - Anuluj wybór **przechowywanie rocznego punktu kopii zapasowej** opcji. Wiodącymi finansów nie chcesz przechowywać dane dłużej niż 36 miesięcy.
    - Kliknij pozycję **OK**, aby utworzyć zasady kopii zapasowych.

    ![Wybieranie obciążenia](./media/tutorial-backup-vm-at-scale/set-new-policy.png) 

    Po utworzeniu zasad tworzenia kopii zapasowej, należy skojarzyć zasady z maszyn wirtualnych.

6. W **wybierz maszyny wirtualne** oknie dialogowym wybierz pozycję *myVM* i kliknij przycisk **OK** Aby wdrożyć zasady tworzenia kopii zapasowych maszyn wirtualnych. 

    Wszystkie maszyny wirtualne, które znajdują się w tej samej lokalizacji, a nie są już skojarzone z zasadami tworzenia kopii zapasowej, są wyświetlane. *myVMH1* i *myVMR1* wybrano ma być skojarzone z *Finance* zasad.

    ![Wybieranie obciążenia](./media/tutorial-backup-vm-at-scale/choose-vm-to-protect.png) 

    Po zakończeniu wdrożenia, zostanie wyświetlone powiadomienie tego wdrożenia pomyślnie ukończona.

## <a name="initial-backup"></a>Początkowa kopia zapasowa

Możliwe jest tworzenie kopii zapasowej dla Magazyny usług odzyskiwania, ale nie została utworzona początkowa kopia zapasowa. Odzyskiwania po awarii najlepsze rozwiązanie do wyzwolenia pierwszej kopii zapasowej jest tak, że dane są chronione. 

Aby uruchomić zadanie tworzenia kopii zapasowej na żądanie:

1. Na pulpicie nawigacyjnym magazynu, kliknij przycisk **3** w obszarze **kopii zapasowej elementów**, aby otworzyć menu elementów kopii zapasowej.

    ![Ikona Ustawienia](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

    **Kopii zapasowej elementów** zostanie otwarte menu.

2. Na **kopii zapasowej elementów** menu, kliknij przycisk **maszyny wirtualnej Azure** aby otworzyć listę maszyny wirtualnej skojarzonej z magazynem.

    ![Ikona Ustawienia](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    Zostanie otwarta lista **Elementy kopii zapasowej**.

    ![Zadanie tworzenia kopii zapasowej zostało wyzwolone](./media/tutorial-backup-vm-at-scale/initial-backup-context-menu.png)

3. Na liście **Elementy kopii zapasowej** kliknij przycisk z wielokropkiem **...**, aby otworzyć menu Kontekst.

4. W menu kontekstowym wybierz **wykonaj kopię zapasową teraz**.

    ![Menu Kontekst](./media/tutorial-backup-vm-at-scale/context-menu.png)

    Zostanie otwarte menu Utwórz kopię zapasową teraz.

5. W menu Utwórz kopię zapasową teraz, wprowadź ostatni dzień, aby zachować punkt odzyskiwania, a następnie kliknij przycisk **kopii zapasowej**.

    ![ustawianie ostatniego dnia okresu zachowywania punktu odzyskiwania Utwórz teraz punkt odzyskiwania](./media/tutorial-backup-vm-at-scale/backup-now-short.png)

    Powiadomienia dotyczące wdrożenia informują o wyzwoleniu zadania tworzenia kopii zapasowej, dzięki czemu możesz monitorować postęp zadania na stronie zadań tworzenia kopii zapasowych. W zależności od rozmiaru maszyny wirtualnej tworzenie początkowej kopii zapasowej może chwilę potrwać.

    Po zakończeniu początkowej zadanie tworzenia kopii zapasowej, widać jego stan w menu zadania tworzenia kopii zapasowej. Zadanie tworzenia kopii zapasowej na żądanie utworzenia punktu przywracania początkowej dla *myVM*. Jeśli chcesz utworzyć kopię zapasową inne maszyny wirtualne, powtórz te czynności dla każdej maszyny wirtualnej. 

    ![Kafelek Zadania tworzenia kopii zapasowej](./media/tutorial-backup-vm-at-scale/initial-backup-complete.png)
  
## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli zamierzasz kontynuować na pracę z kolejnych samouczkach nie oczyszczania zapasowej zasobów utworzonej w tym samouczku. Jeśli nie zamierzasz kontynuować, następujące kroki umożliwiają usunąć wszystkie zasoby utworzone przez tego samouczka w portalu Azure.

1. Na **myRecoveryServicesVault** pulpitu nawigacyjnego, kliknij przycisk **3** w obszarze **kopii zapasowej elementów**, aby otworzyć menu elementów kopii zapasowej.

    ![Ikona Ustawienia](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)


2. Na **kopii zapasowej elementów** menu, kliknij przycisk **maszyny wirtualnej Azure** aby otworzyć listę maszyny wirtualnej skojarzonej z magazynem.

    ![Ikona Ustawienia](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    Zostanie otwarta lista **Elementy kopii zapasowej**.

3. W **kopii zapasowej elementów** menu, kliknij przycisk wielokropka, aby otworzyć menu kontekstowego.

    ![Ikona Ustawienia](./media/tutorial-backup-vm-at-scale/context-menu-to-delete-vm.png)

4. W menu kontekstowym wybierz **kopii zapasowej Zatrzymaj** aby otworzyć menu zatrzymać kopii zapasowej. 

    ![Ikona Ustawienia](./media/tutorial-backup-vm-at-scale/context-menu-for-delete.png)

5. W **zatrzymać kopii zapasowej** menu, wybierz z górnego menu rozwijane i wybierz polecenie **usunąć dane z kopii zapasowej**.

6. W **wpisz nazwę elementu kopii zapasowej** okno dialogowe, typ *myVM*.
 
7. Po zweryfikowaniu elementu kopii zapasowej (znacznik wyboru jest wyświetlana), **kopii zapasowej Zatrzymaj** przycisk jest aktywny. Kliknij przycisk **zatrzymać kopii zapasowej** Aby zatrzymać zasady i usunięcie punktów przywracania. 

    ![Kliknij przycisk Zatrzymaj wykonywanie kopii zapasowej na usunięcie magazynu](./media/tutorial-backup-vm-at-scale/provide-reason-for-delete.png).

8. W **myRecoveryServicesVault** menu, kliknij przycisk **usunąć**.

    ![Kliknij przycisk Zatrzymaj wykonywanie kopii zapasowej na usunięcie magazynu](./media/tutorial-backup-vm-at-scale/deleting-the-vault.png)

    Po usunięciu magazynu, możesz powrócić do listy magazynów usług odzyskiwania.


## <a name="next-steps"></a>Następne kroki

W tym samouczku użyto portalu Azure, aby:

> [!div class="checklist"]
> * Tworzenie magazynu Usług odzyskiwania
> * Ustaw magazynie, aby chronić maszyny wirtualne
> * Utwórz zasadę niestandardową i przechowywania kopii zapasowych
> * Przypisz zasady w celu ochrony wielu maszyn wirtualnych
> * Wyzwalanie na żądanie ponownie dla maszyn wirtualnych

Nadal następny samouczek przywrócenie maszyny wirtualnej platformy Azure z dysku. 

> [!div class="nextstepaction"]
> [Przywracanie maszyn wirtualnych przy użyciu interfejsu wiersza polecenia](./tutorial-restore-disk.md)
