---
title: "Kopia zapasowa Azure — kopia zapasowa Offline lub początkowe umieszczanie za pomocą usługi Import/Eksport Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak Kopia zapasowa Azure umożliwia wysyłanie danych w sieci za pomocą usługi Import/Eksport Azure. W tym artykule opisano, w trybie offline inicjacją dane początkowej kopii zapasowej za pomocą usługi Azure importu wyeksportować."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/18/2017
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: c58aafda21e02e12984e09ef605f7ea13200e381
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Przepływ pracy tworzenia kopii zapasowych w trybie offline w usłudze Azure Backup
Kopia zapasowa Azure zawiera kilka wbudowanych korzyści, które zapisania koszty sieci i magazynu podczas początkowej pełne kopie zapasowe danych na platformie Azure. Początkowa pełnych kopii zapasowych zwykle transfer dużych ilości danych i wymagają większej przepustowości sieci w porównaniu do kolejnych kopii zapasowych, które transfer tylko delty/przyrostowa. Kopia zapasowa Azure kompresuje początkowej kopii zapasowych. W procesie wstępnego wypełniania w trybie offline kopia zapasowa Azure można użyć dysków do przekazania skompresowane dane początkowej kopii zapasowej w trybie offline na platformie Azure.  

Proces w trybie offline wstępne wypełnianie kopia zapasowa Azure jest ściśle zintegrowany z [usługi Import/Eksport Azure](../storage/common/storage-import-export-service.md) który umożliwia przesyłanie danych do platformy Azure przy użyciu dysków. Jeśli masz terabajtów (tabel) dane początkowej kopii zapasowej, który musi być przesyłane przez sieci dużymi opóźnieniami i niskiej przepustowości sieci, można użyć przepływu pracy w trybie offline wstępne wypełnianie na potrzeby wysłania początkowej kopii zapasowej na jeden lub więcej dysków twardych do centrum danych Azure. W tym artykule omówiono czynności, które ukończenie tego przepływu pracy.

## <a name="overview"></a>Przegląd
Z możliwością obsługi w trybie offline Azure Backup i Azure importu/eksportu jest prosty do przekazania danych w trybie offline na platformie Azure przy użyciu dysków. Zamiast transferowania pełnej kopii początkowej przez sieć, dane kopii zapasowej są zapisywane *lokalizacji przejściowej*. Po zakończeniu kopiowania do tymczasowej lokalizacji za pomocą narzędzia Import/Eksport Azure te dane są zapisywane do jednego lub więcej dysków SATA w zależności od ilości danych. Dyski te ostatecznie są wysyłane do najbliższego centrum danych Azure.

[Sierpnia 2016 aktualizacji z usługi Kopia zapasowa Azure (i nowszych)](http://go.microsoft.com/fwlink/?LinkID=229525) obejmuje *narzędzie do przygotowywania dysków Azure*, o nazwie AzureOfflineBackupDiskPrep, który:

* Pomaga przygotować dysków dla importu platformy Azure za pomocą narzędzia Azure importowania/eksportowania.
* Automatycznie tworzy zadania importu platformy Azure dla usługi Import/Eksport Azure w [portalu Azure](https://ms.portal.azure.com).

Po zakończeniu przekazywania danych kopii zapasowej na platformie Azure, kopia zapasowa Azure kopiuje dane kopii zapasowej w magazynie kopii zapasowych i przyrostowych kopii zapasowych.

> [!NOTE]
> Aby korzystać z narzędzia przygotowywania dysków Azure, upewnij się, zainstalowano aktualizację sierpnia 2016 kopia zapasowa Azure (lub nowsza) i wykonaj wszystkie czynności przepływu pracy z nim. Jeśli używasz starszej wersji programu Kopia zapasowa Azure, można przygotować dysk SATA za pomocą narzędzia Import/Eksport Azure zgodnie z opisem w kolejnych sekcjach niniejszego artykułu.
>
>

## <a name="prerequisites"></a>Wymagania wstępne
* [Zapoznaj się z przepływem pracy Import/Eksport Azure](../storage/common/storage-import-export-service.md).
* Przed rozpoczęciem przepływu pracy, należy sprawdzić, czy:
  * Utworzono magazynie usługi Kopia zapasowa Azure.
  * Pobrano poświadczenia magazynu.
  * Agent usługi Kopia zapasowa Azure został zainstalowany na systemu Windows Server/Windows klienta lub serwera programu System Center Data Protection Manager, a komputer jest zarejestrowany w magazynie kopii zapasowej Azure.
* [Pobieranie ustawień publikowania na platformie Azure pliku](https://manage.windowsazure.com/publishsettings) na komputerze, z którego planujesz do tworzenia kopii zapasowej danych.
* Przygotuj tymczasowej lokalizacji, która może być udziału sieciowego lub dodatkowego dysku na komputerze. Tymczasowej lokalizacji przejściowej magazynu i jest używane tymczasowo w tym przepływie pracy. Upewnij się, że w lokalizacji tymczasowej jest za mało miejsca na dysku do przechowywania kopii początkowej. Na przykład jeśli chcesz utworzyć kopię zapasową na serwerze plików 500 GB, upewnij się, że obszaru przemieszczania jest co najmniej 500 GB. (Mniejszą ilość jest używany z powodu kompresji).
* Upewnij się, że dysk obsługiwane. Tylko 2,5 SSD lub 2,5 lub 3,5 cala SATA II/III wewnętrzne dyski twarde są obsługiwane do użycia z usługą importu i eksportu. Dyski twarde można użyć do 10 TB. Sprawdź [dokumentację usługi Import/Eksport Azure](../storage/common/storage-import-export-service.md#hard-disk-drives) najnowszego zestawu dysków, obsługiwanych przez usługę.
* Należy włączyć funkcję BitLocker na komputerze, do którego jest podłączony twórcę dysków SATA.
* [Pobierz narzędzie Import/Eksport Azure](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) do komputera, do którego jest podłączony twórcę dysków SATA. Ten krok nie jest wymagane, jeśli zostały pobrane i zainstalowane aktualizacji sierpnia 2016 kopia zapasowa Azure (lub nowsza).

## <a name="workflow"></a>Przepływ pracy
Informacje przedstawione w tej sekcji pomaga ukończenia przepływu pracy w trybie offline z kopii zapasowej, aby dane można dostarczyć do centrum danych Azure i przekazane do magazynu Azure. Jeśli masz pytania dotyczące usługi importu lub dowolnego aspektu procesu, zobacz [Import — Omówienie usługi](../storage/common/storage-import-export-service.md) dokumentacji odwołuje się do wcześniej.

### <a name="initiate-offline-backup"></a>Inicjowanie kopii zapasowej w trybie offline
1. Podczas planowania kopii zapasowej, zostanie wyświetlony następujący ekran (w systemie Windows Server, klient systemu Windows lub System Center Data Protection Manager).

    ![Importuj ekranu](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Oto odpowiedniego ekranu w System Center Data Protection Manager: <br/>
    ![Program DPM importu ekranu](./media/backup-azure-backup-import-export/dpmoffline.png)

    Opis danych wejściowych jest następujący:

    * **Miejsce przemieszczania**: lokalizacji tymczasowej, do którego początkowa kopia zapasowa jest zapisywany. Może to być na komputerze lokalnym lub w udziale sieciowym. Jeśli kopia komputera i komputera źródłowego są różne, zaleca się czy określić pełną ścieżkę sieciową do lokalizacji tymczasowej.
    * **Nazwa zadania importowania platformy Azure**: unikatową nazwę importu platformy Azure, które usługi i kopia zapasowa Azure śledzić transferu danych przesyłanych na dyskach w systemie Azure.
    * **Ustawień publikowania platformy Azure**: plik XML, który zawiera informacje o profilu subskrypcji. Zawiera ona także bezpiecznych poświadczeń, które są skojarzone z subskrypcją. Możesz [Pobierz plik](https://manage.windowsazure.com/publishsettings). Podaj ścieżkę lokalną do pliku ustawień publikowania.
    * **Identyfikator subskrypcji platformy Azure**: identyfikator subskrypcji platformy Azure dla subskrypcji, w których planuje się zainicjowanie zadania importu platformy Azure. Jeśli masz wiele subskrypcji Azure, należy użyć Identyfikatora subskrypcji, która ma zostać skojarzona z zadaniem importu.
    * **Konto usługi Azure Storage**: Konto magazynu w subskrypcji platformy Azure skojarzone z zadaniem importu platformy Azure.
    * **Kontener magazynu Azure**: Nazwa magazynu docelowego obiektu blob na koncie magazynu Azure, gdzie importowania danych z tego zadania.

    > [!NOTE]
    > Jeśli zarejestrowano serwer w magazynie usług odzyskiwania Azure z [portalu Azure](https://portal.azure.com) dla kopii zapasowych i nie są w subskrypcji Cloud Solution Provider (CSP), można tworzyć konta magazynu z portalu Azure i Użyj go przepływu pracy w trybie offline z kopii zapasowej.
    >
    >

     Te informacje należy zapisać, ponieważ należy wprowadzić go ponownie w poniższych krokach. Tylko *lokalizacji przejściowej* jest wymagany, jeśli jest używane narzędzie do przygotowywania dysków Azure przygotować dyski.    
2. Ukończenia przepływu pracy, a następnie wybierz **wykonaj kopię zapasową teraz** w konsoli zarządzania programu Kopia zapasowa Azure, aby zainicjować kopii zapasowej w trybie offline. Tworzenie początkowej kopii zapasowej są zapisywane do obszaru przemieszczania w ramach tego kroku.

    ![Wykonaj kopię zapasową](./media/backup-azure-backup-import-export/backupnow.png)

    Do ukończenia odpowiedniego przepływu pracy w System Center Data Protection Manager, kliknij prawym przyciskiem myszy **grupy ochrony**, a następnie wybierz pozycję **Utwórz punkt odzyskiwania** opcji. Następnie wybierz pozycję **ochrony w trybie Online** opcji.

    ![Natychmiastowe tworzenie kopii zapasowej programu DPM](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Po zakończeniu operacji, przemieszczana lokalizacja jest gotowy do użycia w celu przygotowania dysku.

    ![Postęp tworzenia kopii zapasowej](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-a-sata-drive-and-create-an-azure-import-job-by-using-the-azure-disk-preparation-tool"></a>Przygotowanie dysków SATA i Utwórz zadania importu platformy Azure za pomocą narzędzia przygotowywania dysków Azure
Narzędzie do przygotowywania dysków Azure jest dostępny w katalogu instalacji agenta usług odzyskiwania (aktualizacja z sierpnia 2016 lub nowszy) w następującej ścieżce.

   *\Microsoft* *azure* *odzyskiwania* *usług* * Agent\Utils\*

1. Przejdź do katalogu, a następnie skopiuj **AzureOfflineBackupDiskPrep** katalogu na komputerze kopiowania, na którym są zainstalowane dyski, aby móc przywrócić. Upewnij się, poniżej w odniesieniu do komputera kopiowania:

    * Komputer kopii można uzyskać dostęp do tymczasowej lokalizacji dla przepływu pracy w trybie offline wstępne wypełnianie przy użyciu tej samej lokalizacji sieciowej, która została dostarczona w **zainicjować kopię zapasową offline** przepływu pracy.
    * Funkcja BitLocker jest włączona na komputerze.
    * Komputer może uzyskać dostępu do portalu Azure.

    W razie potrzeby kopiowania komputer może być taka sama, co na komputerze źródłowym.
2. Otwórz wiersz polecenia z podwyższonym poziomem uprawnień na komputerze kopiowania z katalogiem narzędzia Azure przygotowanie dysku jako bieżący katalog i uruchom następujące polecenie:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to PublishSettingsFile*>]`

    | Parametr | Opis |
    | --- | --- |
    | s:&lt;*ścieżka lokalizacji przemieszczania*&gt; |Wymagane dane wejściowe, służący do Podaj ścieżkę do lokalizacji tymczasowej, wprowadzony w **zainicjować kopię zapasową offline** przepływu pracy. |
    | p:&lt;*ścieżkę do PublishSettingsFile*&gt; |Opcjonalne dane wejściowe, służący do Podaj ścieżkę do **ustawień publikowania platformy Azure** pliku wprowadzony w **zainicjować kopię zapasową offline** przepływu pracy. |

    > [!NOTE]
    > &lt;Ścieżkę do PublishSettingFile&gt; wartość jest wymagany, jeśli kopia komputera i komputera źródłowego są różne.
    >
    >

    Po uruchomieniu polecenia narzędzia żądań wybór zadania importu platformy Azure, umożliwiająca dyski, które muszą być przygotowane. Jeśli tylko zadania importu pojedynczego jest skojarzony z podanej lokalizacji tymczasowej, zostanie wyświetlony ekran podobny do tego, który jest zgodny.

    ![Azure wprowadzania narzędzie przygotowanie dysku](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Wprowadź literę dysku bez dwukropka końcowe dla zainstalowanego dysku, który ma zostać przygotowane do przeniesienia do usługi Azure. Potwierdź formatowania dysku po wyświetleniu monitu.

    Narzędzie następnie rozpoczyna się przygotować dysk z kopii zapasowej danych. Może być konieczne dołączanie dodatkowych dysków, po wyświetleniu monitu przez narzędzie, w przypadku udostępnionego dysku nie ma wystarczającą ilość miejsca na dane kopii zapasowej. <br/>

    Na koniec pomyślnego wykonania narzędzia jeden lub więcej dysków, które są przygotowywane do wysyłki na platformie Azure. Ponadto zadania importu o nazwie podany podczas **zainicjować kopię zapasową offline** przepływu pracy jest tworzony w portalu Azure. Na koniec narzędzie wyświetla adres wysyłkowy do centrum danych Azure, której dyski muszą być wysłane i łącza do zlokalizowania zadania importu w portalu Azure.

    ![Zakończenie przygotowywania dysku platformy Azure](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Wysłać dysków na adres zapewnianej przez narzędzie i Zachowaj numer śledzenia do użytku w przyszłości.<br/>

5. Po przejściu do łącza wyświetlany w narzędziu, zobacz konto magazynu Azure, który określono w **zainicjować kopię zapasową offline** przepływu pracy. W tym miejscu można wyświetlić zadania importu nowo utworzony na **IMPORTU/eksportu** kartę konta magazynu.

    ![Zadania utworzone importu](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>

6. Kliknij przycisk **WYSYŁANIA informacji o** w dolnej części strony, aby zaktualizować swoje szczegóły dotyczące kontaktu, jak pokazano na poniższym ekranie. Firma Microsoft używa tych informacji do wydania dysków powrotem po zakończeniu zadania importu.

    ![Informacje kontaktowe](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>

7. Wprowadź szczegóły wysyłki na następnym ekranie. Podaj **operatora dostarczania** i **numer identyfikacyjny** szczegółowe informacje, które odpowiadają na dyskach, które są wysyłane do centrum danych Azure.

    ![Wysyłanie informacji o](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### <a name="complete-the-workflow"></a>Ukończenia przepływu pracy
Po zakończeniu zadania importu danych początkowej kopii zapasowej jest dostępna na koncie magazynu. Agent usług odzyskiwania, a następnie kopiuje zawartość dane z tego konta do magazynu kopii zapasowej lub odzyskiwania usługi Magazyn, którekolwiek ma zastosowanie. W następnym zaplanowanym terminie kopii zapasowej agenta kopii zapasowej Azure wykonuje przyrostowej kopii zapasowej za pośrednictwem początkowa kopia zapasowa.

> [!NOTE]
> Poniższe sekcje dotyczą użytkowników wcześniejszych wersjach programu Kopia zapasowa Azure, którzy nie mają dostępu do narzędzia przygotowywania dysków Azure.
>
>

### <a name="prepare-a-sata-drive"></a>Przygotowanie dysków SATA
1. Pobierz [narzędzie importu/eksportu pakietu Microsoft Azure](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) komputerowi kopiowania. Upewnij się, że przemieszczana lokalizacja jest dostępny z komputera, w którym ma zostać uruchomiona dalej zestaw poleceń. W razie potrzeby kopiowania komputer może być taka sama, co na komputerze źródłowym.

2. Rozpakuj plik WAImportExport.zip. Uruchom narzędzie WAImportExport formaty dysków SATA, zapisuje dane kopii zapasowej na dysku SATA i szyfruje go. Przed uruchom następujące polecenie, upewnij się, że funkcja BitLocker jest włączona na komputerze. <br/>

    `*.\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*staging location*> /dstdir: <*DestinationBlobVirtualDirectory*>/*`

    > [!NOTE]
    > Po zainstalowaniu aktualizacji sierpnia 2016 kopia zapasowa Azure (lub nowsza), upewnij się, że wprowadzonej lokalizacji tymczasowej jest taka sama, jak na **wykonaj kopię zapasową teraz** ekranu i zawiera pliki AIB i obiektów Blob Base.
    >
    >

| Parametr | Opis |
| --- | --- |
| /j: <*JournalFile*> |Ścieżka do pliku dziennika. Każdy dysk musi mieć dokładnie jeden plik dziennika. Plik dziennika nie może być na dysku docelowym. Rozszerzenie pliku dziennika jest .jrn i zostanie utworzony jako część tego polecenia. |
| / Identyfikator: <*SessionId*> |Identyfikator sesji określa sesji kopiowania. Służy do dokładnych odzyskiwanie kopii przerwania sesji. Pliki, które są kopiowane w ramach sesji kopiowania są przechowywane w katalogu o nazwie po identyfikator sesji na docelowym dysku. |
| /SK: <*StorageAccountKey*> |Klucz konta dla konta magazynu, do którego będą importowane dane. Klucz musi być taka sama, jak została podana podczas tworzenia grupy ochrony zasad tworzenia kopii zapasowej. |
| / BlobType |Typ obiektu blob. Ten przepływ pracy zakończy się powodzeniem, tylko wtedy, gdy **PageBlob** jest określona. To nie jest opcją domyślną i powinny być podane w tego polecenia. |
| / t: <*TargetDriveLetter*> |Litera dysku bez końcowych dwukropkiem docelowego dysku twardego dla bieżącej sesji kopiowania. |
| / Format |Opcja do sformatowania dysku. Określ ten parametr, gdy dysk musi być sformatowany; w przeciwnym razie Pomiń go. Zanim narzędzie formatuje dysk, wyświetli monit o potwierdzenie w konsoli. Aby pominąć potwierdzenie, należy określić parametr /silentmode. |
| / szyfrowania |Opcja szyfrowania dysku. Określ ten parametr, gdy dysku nie ma jeszcze zaszyfrowany za pomocą funkcji BitLocker i musi być szyfrowane przez narzędzie. Jeśli dysk już został zaszyfrowany za pomocą funkcji BitLocker, Pomiń ten parametr, określ parametr /bk i podaj istniejącego klucza funkcji BitLocker. Jeśli zostanie użyty parametr/format, należy również określić / zaszyfrować parametru. |
| /srcdir: <*SourceDirectory*> |Katalog źródłowy, który zawiera pliki, które ma zostać skopiowany na dysk docelowy. Sprawdź, czy nazwa określonego katalogu ma pełne zamiast względną ścieżkę. |
| /dstdir: <*DestinationBlobVirtualDirectory*> |Ścieżka do katalogu wirtualnego docelowego na koncie magazynu Azure. Należy użyć nazwy kontenera prawidłowe po określeniu katalogi wirtualne docelowego lub obiektów blob. Należy pamiętać, że nazwy kontenerów muszą być małymi literami.  Ta nazwa kontenera powinien być wprowadzona podczas tworzenia grupy ochrony zasad tworzenia kopii zapasowej. |

> [!NOTE]
> Plik dziennika jest tworzony w folderze WAImportExport, które znajdują się informacje dotyczące całego przepływu pracy. Należy korzystać z tego pliku podczas tworzenia zadania importu w portalu Azure.
>
>

  ![Dane wyjściowe programu PowerShell](./media/backup-azure-backup-import-export/psoutput.png)

### <a name="create-an-import-job-in-the-azure-portal"></a>Utwórz zadanie importu w portalu Azure
1. Przejdź do swojego konta magazynu w [portalu Azure](https://ms.portal.azure.com/), kliknij przycisk **importu/eksportu**, a następnie **Tworzenie zadania importu** w okienku zadań.

    ![Karta importu/eksportu w portalu Azure](./media/backup-azure-backup-import-export/azureportal.png)

2. W kroku 1 Kreatora wskazują przygotowaniu dysku i czy masz pliku dziennika dysku jest dostępna.

3. W kroku 2 kreatora podaj informacje kontaktowe dla osoby, która jest odpowiedzialna za tego zadania importu.

4. W kroku 3 przekazywanie plików dziennika dysków, które uzyskane w poprzedniej sekcji.

5. W kroku 4 wprowadź nazwę opisową dla zadania importu wprowadzonej podczas tworzenia grupy ochrony zasad tworzenia kopii zapasowej. Wprowadzona nazwa może zawierać tylko małe litery, cyfry, łączniki i podkreślenia, musi zaczynać się literą i nie może zawierać spacji. Nazwę, którą można wybrać służy do śledzenia zadań, gdy są one w toku, a po ich wykonaniu.

6. Następnie wybierz region centrum danych z listy. Region centrum danych wskazuje centrum danych i adres, do którego należy wysłać pakietu.

    ![Wybierz region, w centrum danych](./media/backup-azure-backup-import-export/dc.png)

7. W kroku 5 Wybierz zwracany operatora z listy, a następnie wprowadź numer swojego konta operatora. Firma Microsoft używa tego konta na potrzeby wysłania dysków powrotem po zakończeniu zadania importu.

8. Wyślij dysk, a następnie wprowadź numer do śledzenia stanu wydania. Po odebraniu dysku w centrum danych jest kopiowana do konta magazynu, a stan zostanie zaktualizowany.

    ![Stan zakończenia](./media/backup-azure-backup-import-export/complete.png)

### <a name="complete-the-workflow"></a>Ukończenia przepływu pracy
Po dane początkowej kopii zapasowej jest dostępny na koncie magazynu, agent usług odzyskiwania Microsoft Azure kopiuje zawartość dane z tego konta do magazynu kopii zapasowej lub magazyn usług odzyskiwania, którekolwiek ma zastosowanie. W następnym harmonogramu wykonywania kopii zapasowej agenta kopii zapasowej Azure wykonuje przyrostowej kopii zapasowej za pośrednictwem początkowa kopia zapasowa.

## <a name="next-steps"></a>Kolejne kroki
* Wszelkie pytania dotyczące przepływu pracy Import/Eksport Azure można znaleźć w temacie [transferu danych do magazynu obiektów Blob za pomocą usługi Import/Eksport Microsoft Azure](../storage/common/storage-import-export-service.md).
* Zapoznaj się z sekcją w trybie offline z kopii zapasowej systemu Azure Backup [— często zadawane pytania](backup-azure-backup-faq.md) wszelkie pytania dotyczące przepływu pracy.
