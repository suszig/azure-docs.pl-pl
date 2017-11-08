---
title: "Przywróć dane do systemu Windows Server lub klienta systemu Windows z platformy Azure przy użyciu klasycznego modelu wdrażania | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przywracanie z systemu Windows Server lub klienta systemu Windows."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 300b2b17b44e21ed446fd63d572a2461e2fc1343
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-the-classic-deployment-model"></a>Przywracanie plików do maszyny z systemem Windows Server lub Client przy użyciu klasycznego modelu wdrażania
> [!div class="op_single_selector"]
> * [Portal klasyczny](backup-azure-restore-windows-server-classic.md)
> * [Witryna Azure Portal](backup-azure-restore-windows-server.md)
>
>

W tym artykule wyjaśniono, jak odzyskać dane z magazynu kopii zapasowej i przywrócenie go do serwera lub komputera. Począwszy od 2017 marca można już utworzyć magazynów kopii zapasowych w klasycznym portalu.

> [!IMPORTANT]
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> **15 października 2017 r.**: nie będzie już można tworzyć magazynów kopii zapasowych przy użyciu programu PowerShell. <br/> **Od 1 listopada 2017 roku**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
>

Aby przywrócić dane, używasz Kreatora odzyskiwania danych w agencie usług odzyskiwania Azure firmy Microsoft (MARS). Po przywróceniu danych, istnieje możliwość:

* Przywróć dane do tego samego komputera, z którego wykonano kopie zapasowe.
* Przywracanie danych do alternatywnej maszyny.

W stycznia 2017 r firma Microsoft opublikowała Podgląd aktualizacji agenta MARS. Wraz z poprawki ta aktualizacja umożliwia błyskawiczne przywrócić, dzięki czemu można zainstalować migawkę punktu odzyskiwania zapisywalny, jako wolumin odzyskiwania. Następnie można eksplorować odzyskiwania woluminu i kopiować pliki na komputerze lokalnym, a tym samym selektywne Przywracanie plików.

> [!NOTE]
> [Stycznia 2017 aktualizacji kopia zapasowa Azure](https://support.microsoft.com/en-us/help/3216528?preview) jest wymagany, jeśli chcesz użyć błyskawicznych przywracania, aby przywrócić dane. Również dane kopii zapasowej muszą być chronione w magazynach przy ustawieniach regionalnych wymienione w artykule pomocy technicznej. Zapoznaj się [stycznia 2017 aktualizacji kopia zapasowa Azure](https://support.microsoft.com/en-us/help/3216528?preview) najbardziej aktualną listę ustawień regionalnych, obsługujących błyskawicznych przywracania. Przywracanie błyskawicznych jest **nie** dostępne dla wszystkich ustawień regionalnych.
>

Błyskawicznych Przywracanie jest dostępne do użycia w Magazyny usług odzyskiwania w portalu Azure i magazyny kopii zapasowych w klasycznym portalu. Jeśli chcesz użyć błyskawicznych przywracania, Pobierz aktualizację MARS i postępuj zgodnie z procedurami, których występuje błyskawicznych przywracania.


## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Użyj błyskawicznych przywracania, aby odzyskać dane do tej samej maszyny

Jeśli przypadkowo usunięto plik i chcesz przywrócić go do tego samego komputera (z którego jest pobierany kopii zapasowej), poniższe kroki pomoże Ci odzyskania danych.

1. Otwórz **kopia zapasowa Microsoft Azure** przyciągania w. Jeśli nie znasz, którym zainstalowano przystawkę, wyszukiwanie w komputerze lub serwerze dla **kopia zapasowa Microsoft Azure**.

    Aplikacja klasyczna powinny być wyświetlane w wynikach wyszukiwania.

2. Kliknij przycisk **odzyskać dane** Aby uruchomić kreatora.

    ![Odzyskiwanie danych](./media/backup-azure-restore-windows-server/recover.png)

3. Na **wprowadzenie** okienko, aby przywrócić dane do tego samego serwera lub komputera, wybierz **tego serwera (`<server name>`)** i kliknij przycisk **dalej**.

    ![Wybierz tę opcję serwera, aby przywrócić dane do tej samej maszyny](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Na **wybierz tryb odzyskiwania** okienku wybierz **poszczególnych plików i folderów** , a następnie kliknij przycisk **dalej**.

    ![Przeglądanie plików](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. Na **Wybierz wolumin i data** okienku, wybierz wolumin, który zawiera pliki lub foldery, które mają zostać przywrócone.

    W kalendarzu wybierz punkt odzyskiwania. Można przywrócić z dowolnego punktu odzyskiwania w czasie. Daty w **bold** wskazuje dostępność co najmniej jeden punkt odzyskiwania. Po wybraniu Data, jeśli dostępnych jest wiele punktów odzyskiwania, należy wybrać określony punkt odzyskiwania z **czasu** menu rozwijanego.

    ![Data i woluminu](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Kliknij wybrany punkt odzyskiwania, aby przywrócić **instalacji**.

    Kopia zapasowa Azure instaluje punkt odzyskiwania lokalnych i używa go jako wolumin odzyskiwania.

7. Na **przeglądania i odzyskiwanie plików** okienku, kliknij przycisk **Przeglądaj** Otwórz Eksploratora Windows i wyszukiwanie plików i folderów.

    ![Opcje odzyskiwania](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. W Eksploratorze Windows skopiuj pliki lub foldery, które chcesz przywrócić i wklej je do dowolnej lokalizacji do serwera lub komputera lokalnego. Można otworzyć lub strumienia pliki bezpośrednio z woluminu odzyskiwania i sprawdź, czy poprawne wersje są odzyskiwane.

    ![Skopiuj i Wklej pliki i foldery z zainstalowanego woluminu do lokalizacji lokalnej](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Po zakończeniu przywracania plików i/lub folderów, na **przeglądania i pliki odzyskiwania** okienku, kliknij przycisk **Odinstaluj**. Następnie kliknij przycisk **tak** potwierdzenie dezinstalacji woluminu.

    ![Dezinstalacji woluminu i Potwierdź](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Jeśli nie kliknij polecenie Odinstaluj, wolumin odzyskiwania pozostaną zainstalowanych przez 6 godzin od momentu, gdy został zainstalowany. Nie operacji tworzenia kopii zapasowej zostanie uruchomiony, gdy wolumin jest zainstalowany. Żadnej operacji tworzenia kopii zapasowej zaplanowane do uruchomienia w czasie, gdy wolumin jest zainstalowany, zostanie uruchomiony po odinstalowane woluminu odzyskiwania.
    >


## <a name="recover-data-to-the-same-machine"></a>Odzyskiwanie danych na tym samym komputerze
Jeśli przypadkowo usunięto plik i chcesz przywrócić go do tego samego komputera (z którego jest pobierany kopii zapasowej), poniższe kroki pomoże Ci odzyskania danych.

1. Otwórz **kopia zapasowa Microsoft Azure** przyciągania w.
2. Kliknij przycisk **odzyskać dane** do inicjowania przepływu pracy.

    ![Odzyskiwanie danych](./media/backup-azure-restore-windows-server-classic/recover.png)
3. Wybierz  **tego serwera (*yourmachinename*) ** opcję, aby przywrócić kopię zapasową pliku na tym samym komputerze.

    ![Tym samym komputerze](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. Wybierz **Przeglądaj w poszukiwaniu plików** lub **wyszukiwania plików**.

    Pozostaw opcję domyślną, jeśli zamierzasz przywrócić jeden lub więcej plików, których ścieżką jest znany. Jeśli nie pewności o struktury folderów, ale chcesz wyszukać plik, wybierz **wyszukiwania plików** opcji. Na potrzeby tej sekcji firma Microsoft będzie kontynuowana z domyślną opcją.

    ![Przeglądanie plików](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. Wybierz wolumin, z którego chcesz przywrócić plik.

    Można przywrócić z dowolnego punktu w czasie. Daty, które są wyświetlane w **bold** w formancie kalendarza wskazuje dostępność punktu przywracania. Po wybraniu daty na podstawie harmonogramu tworzenia kopii zapasowych (i powodzenie operacji tworzenia kopii zapasowej), można wybrać punkt w czasie z **czasu** listy rozwijanej.

    ![Data i woluminu](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. Wybierz elementy do odzyskania. Możesz wybrać wiele folderów/plików, który chcesz przywrócić.

    ![Wybieranie pozycji Pliki](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. Określ parametry odzyskiwania.

    ![Opcje odzyskiwania](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * Istnieje możliwość przywracania do oryginalnej lokalizacji (w którym plik lub folder, zostaną zastąpione) lub w innej lokalizacji, w tym samym komputerze.
   * Jeśli istnieje plik lub folder, który chcesz przywrócić w lokalizacji docelowej, można utworzyć kopie (dwie wersje tego samego pliku), zastąpienie plików w lokalizacji docelowej lub Pomiń odzyskiwanie plików, które istnieją w obiekcie docelowym.
   * Zdecydowanie zaleca się pozostawienie domyślną opcją Przywracanie listy kontroli dostępu do plików, które są odzyskiwane.
8. Gdy te dane wejściowe są dostarczane, kliknij przycisk **dalej**. Przepływ pracy odzyskiwania, które przywraca pliki z tą maszyną, rozpocznie się.

## <a name="recover-to-an-alternate-machine"></a>Odzyskiwanie do alternatywnej maszyny
W przypadku utraty cały serwer można jednak odzyskać dane z kopii zapasowej Azure na innym komputerze. Poniższe kroki przedstawiają przepływ pracy.  

Składa się z terminologią używaną w następujące kroki:

* *Maszyna źródłowa* — oryginalnego komputera, z którego wykonano kopię zapasową i który jest obecnie niedostępny.
* *Maszyna docelowa* — komputera, do którego dane są odzyskiwane.
* *Przykładowe magazynu* — magazyn kopii zapasowych, do którego *maszyny źródłowej* i *maszyny docelowej* są rejestrowane. <br/>

> [!NOTE]
> Nie można przywrócić kopii zapasowych wykonanych na komputerze, na komputerze, na którym jest uruchomiony program wcześniejszej wersji systemu operacyjnego. Na przykład jeśli kopie zapasowe są wykonywane z komputera z systemem Windows 7, jego można przywracać w systemie Windows 8 lub nowszym maszyny. Jednak nie ma wartość true na odwrót.
>
>

1. Otwórz **kopia zapasowa Microsoft Azure** przyciągania w na *maszyny docelowej*.
2. Upewnij się, że *maszyny docelowej* i *maszyny źródłowej* są rejestrowane w tym samym magazynie kopii zapasowych.
3. Kliknij przycisk **odzyskać dane** do inicjowania przepływu pracy.

    ![Odzyskiwanie danych](./media/backup-azure-restore-windows-server-classic/recover.png)
4. Wybierz **innego serwera**

    ![Inny serwer](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. Podaj plik poświadczeń magazynu, który odpowiada *magazynu próbki*. Jeśli plik poświadczeń magazynu jest nieprawidłowa (lub wygasła), Pobierz nowy plik poświadczeń magazynu z *magazynu próbki* w klasycznym portalu Azure. Po podany plik poświadczeń magazynu na magazyn kopii zapasowych przed plik poświadczeń magazynu jest wyświetlana.
6. Wybierz *maszyny źródłowej* z listy wyświetlanych maszyn.

    ![Lista komputerów](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. Wybierz opcję **wyszukiwania plików** lub **Przeglądaj w poszukiwaniu plików** opcji. Na potrzeby tej sekcji użyjemy **wyszukiwania plików** opcji.

    ![Wyszukiwanie](./media/backup-azure-restore-windows-server-classic/search.png)
8. Wybierz wolumin i Data na następnym ekranie. Wyszukaj nazwę pliku/folderu, który chcesz przywrócić.

    ![Wyszukiwanie elementów](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. Wybierz lokalizację, w których konieczne jest przywrócenie plików.

    ![Przywracanie w lokalizacji](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. Podaj hasło szyfrowania, które zostało podane podczas *maszyny źródłowej* rejestracji *magazynu próbki*.

    ![Szyfrowanie](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. Po danych wejściowych jest dostępne, kliknij przycisk **odzyskać**, który wyzwala przywracania kopii zapasowej plików do lokalizacji docelowej, pod warunkiem.

## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Umożliwia błyskawiczne przywrócić przywracania danych do alternatywnej maszyny
W przypadku utraty cały serwer można jednak odzyskać dane z kopii zapasowej Azure na innym komputerze. Poniższe kroki przedstawiają przepływ pracy.

Składa się z terminologią używaną w następujące kroki:

* *Maszyna źródłowa* — oryginalnego komputera, z którego wykonano kopię zapasową i który jest obecnie niedostępny.
* *Maszyna docelowa* — komputera, do którego dane są odzyskiwane.
* *Przykładowe magazynu* — magazyn usług odzyskiwania i do którego *maszyny źródłowej* i *maszyny docelowej* są rejestrowane. <br/>

> [!NOTE]
> Nie można przywrócić kopii zapasowych na komputerze docelowym, ze starszą wersją systemu operacyjnego. Na przykład kopii zapasowej z systemu Windows 7, komputer może być przywrócona w systemie Windows 8 lub nowszym, komputer. Nie można przywrócić kopii zapasowej z komputera z systemem Windows 8 na komputerze z systemem Windows 7.
>
>

1. Otwórz **kopia zapasowa Microsoft Azure** przyciągania w na *maszyny docelowej*.

2. Upewnij się, *maszyny docelowej* i *maszyny źródłowej* są rejestrowane w tym samym magazynie usług odzyskiwania.

3. Kliknij przycisk **odzyskać dane** otworzyć **Kreatora odzyskiwania danych**.

    ![Odzyskiwanie danych](./media/backup-azure-restore-windows-server/recover.png)

4. Na **wprowadzenie** okienku wybierz **innego serwera**

    ![Inny serwer](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Podaj plik poświadczeń magazynu, który odpowiada *magazynu próbki*i kliknij przycisk **dalej**.

    Jeśli plik poświadczeń magazynu jest nieprawidłowa (lub wygasła), Pobierz nowy plik poświadczeń magazynu z *magazynu próbki* w portalu Azure. Po podaniu poświadczeń ważny magazyn, zostanie wyświetlony nazwę odpowiedniego magazynu kopii zapasowej.

6. Na **wybierz serwer zapasowy** okienku, wybierz *maszyny źródłowej* z listy wyświetlanych maszyn i podaj hasło. Następnie kliknij przycisk **Next** (Dalej).

    ![Lista komputerów](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Na **wybierz tryb odzyskiwania** okienku, wybierz **poszczególnych plików i folderów** i kliknij przycisk **dalej**.

    ![Wyszukiwanie](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Na **Wybierz wolumin i data** okienku, wybierz wolumin, który zawiera pliki lub foldery, które mają zostać przywrócone.

    W kalendarzu wybierz punkt odzyskiwania. Można przywrócić z dowolnego punktu odzyskiwania w czasie. Daty w **bold** wskazuje dostępność co najmniej jeden punkt odzyskiwania. Po wybraniu Data, jeśli dostępnych jest wiele punktów odzyskiwania, należy wybrać określony punkt odzyskiwania z **czasu** menu rozwijanego.

    ![Wyszukiwanie elementów](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Kliknij przycisk **instalacji** można lokalnie zainstalować jako woluminu odzyskiwania punkt odzyskiwania na Twojej *maszyny docelowej*.

10. Na **przeglądania i odzyskiwanie plików** okienku, kliknij przycisk **Przeglądaj** Otwórz Eksploratora Windows i wyszukiwanie plików i folderów.

    ![Szyfrowanie](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. W Eksploratorze Windows, skopiuj pliki lub foldery z tego woluminu, odzyskiwania i wklej je do Twojej *maszyny docelowej* lokalizacji. Można otworzyć lub strumienia pliki bezpośrednio z woluminu odzyskiwania i sprawdź, czy poprawne wersje są odzyskiwane.

    ![Szyfrowanie](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Po zakończeniu przywracania plików i/lub folderów, na **przeglądania i pliki odzyskiwania** okienku, kliknij przycisk **Odinstaluj**. Następnie kliknij przycisk **tak** potwierdzenie dezinstalacji woluminu.

    ![Szyfrowanie](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Jeśli nie kliknij polecenie Odinstaluj, wolumin odzyskiwania pozostaną zainstalowanych przez 6 godzin od momentu, gdy został zainstalowany. Nie operacji tworzenia kopii zapasowej zostanie uruchomiony, gdy wolumin jest zainstalowany. Żadnej operacji tworzenia kopii zapasowej zaplanowane do uruchomienia w czasie, gdy wolumin jest zainstalowany, zostanie uruchomiony po odinstalowane woluminu odzyskiwania.
    >


## <a name="next-steps"></a>Następne kroki
* [Azure — często zadawane pytania kopii zapasowej](backup-azure-backup-faq.md)
* Odwiedź stronę [Forum kopii zapasowej Azure](http://go.microsoft.com/fwlink/p/?LinkId=290933).

## <a name="learn-more"></a>Dowiedz się więcej
* [Omówienie kopii zapasowych Azure](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [Kopii zapasowych maszyn wirtualnych platformy Azure](backup-azure-vms-introduction.md)
* [Wykonywanie kopii zapasowej obciążeń firmy Microsoft](backup-azure-dpm-introduction.md)
