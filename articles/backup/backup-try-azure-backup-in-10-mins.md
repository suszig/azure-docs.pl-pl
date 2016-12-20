---
title: "Dowiedz się, jak tworzyć kopie zapasowe plików i folderów z systemu Windows na platformie Azure za pomocą usługi Azure Backup przy użyciu modelu wdrażania usługi Resource Manager | Microsoft Docs"
description: "Dowiedz się, jak utworzyć kopię zapasową danych systemu Windows Server przez utworzenie magazynu, zainstalowanie agenta Usług odzyskiwania i wykonanie kopii zapasowej plików i folderów na platformie Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keywords: "jak tworzyć kopie zapasowe; tworzenie kopii zapasowych"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/27/2016
ms.author: markgal;
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 8ac37244d1e691a2f075116f3d78b89923cfb845


---
# <a name="first-look-back-up-files-and-folders-with-azure-backup-using-the-resource-manager-deployment-model"></a>Pierwsze spojrzenie: tworzenie kopii zapasowych plików i folderów za pomocą usługi Azure Backup przy użyciu modelu wdrażania usługi Resource Manager
W tym artykule opisano sposób wykonywania kopii zapasowej plików i folderów systemu Windows Server (lub klienta systemu Windows) na platformie Azure za pomocą usługi Azure Backup przy użyciu usługi Resource Manager. Ten samouczek zawiera podstawowe informacje. Jeśli chcesz rozpocząć korzystanie z usługi Azure Backup, to jesteś w odpowiednim miejscu.

Jeśli chcesz dowiedzieć się więcej o usłudze Azure Backup, przeczytaj to [omówienie](backup-introduction-to-azure-backup.md).

Do utworzenia kopii zapasowej plików i folderów na platformie Azure wymagane jest wykonanie następujących działań:

![Krok 1](./media/backup-try-azure-backup-in-10-mins/step-1.png). Uzyskiwanie subskrypcji platformy Azure (jeśli jeszcze jej nie masz)<br>
![Krok 2](./media/backup-try-azure-backup-in-10-mins/step-2.png). Tworzenie magazynu usługi Recovery Services<br>
![Krok 3](./media/backup-try-azure-backup-in-10-mins/step-3.png). Pobieranie niezbędnych plików<br>
![Krok 4](./media/backup-try-azure-backup-in-10-mins/step-4.png). Instalowanie i rejestrowanie agenta usługi Recovery Services<br>
![Krok 5](./media/backup-try-azure-backup-in-10-mins/step-5.png). Tworzenie kopii zapasowej plików i folderów

![Tworzenie kopii zapasowej komputera z systemem Windows za pomocą usługi Azure Backup](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

## <a name="step-1-get-an-azure-subscription"></a>Krok 1. Uzyskiwanie subskrypcji platformy Azure
Jeśli nie masz subskrypcji platformy Azure, utwórz [bezpłatne konto](https://azure.microsoft.com/free/) umożliwiające dostęp do dowolnej usługi Azure.

## <a name="step-2-create-a-recovery-services-vault"></a>Krok 2. Tworzenie magazynu Usług odzyskiwania
Aby utworzyć kopię zapasową plików i folderów, należy utworzyć magazyn Usług odzyskiwania w regionie, w którym chcesz przechowywać dane. Należy również określić sposób replikowania magazynu.

### <a name="to-create-a-recovery-services-vault"></a>Aby utworzyć magazyn Usług odzyskiwania
1. Jeśli nie zostało to wcześniej zrobione, zaloguj się do [Portalu Azure](https://portal.azure.com/) przy użyciu subskrypcji platformy Azure.
2. W menu Centrum kliknij pozycję **Przeglądaj**, na liście zasobów wpisz łańcuch **Usługi odzyskiwania**, a następnie kliknij pozycję **Magazyny Usług odzyskiwania**.
   
    ![Tworzenie magazynu Usług odzyskiwania — krok 1](./media/backup-try-azure-backup-in-10-mins/browse-to-rs-vaults.png) <br/>
3. W menu **Magazyny Usług odzyskiwania** kliknij pozycję **Dodaj**.
   
    ![Tworzenie magazynu Usług odzyskiwania — krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)
   
    Zostanie otwarty blok magazynu Usług odzyskiwania i pojawi się monit o podanie wartości w polach **Nazwa**, **Subskrypcja**, **Grupa zasobów** i **Lokalizacja**.
   
    ![Tworzenie magazynu Usług odzyskiwania — krok 5](./media/backup-try-azure-backup-in-10-mins/rs-vault-attributes.png)
4. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn.
5. Kliknij pozycję **Subskrypcja**, aby wyświetlić listę dostępnych subskrypcji.
6. Kliknij pozycję **Grupa zasobów**, aby wyświetlić listę dostępnych grup zasobów, lub kliknij pozycję **Nowa**, aby utworzyć nową grupę zasobów.
7. Kliknij pozycję **Lokalizacja**, aby wybrać region geograficzny magazynu. Ten wybór określa region geograficzny, do którego wysyłane są dane kopii zapasowej.
8. Kliknij przycisk **Utwórz**.
   
    Jeśli nie widzisz swojego magazynu po zakończeniu jego tworzenia, kliknij pozycję **Odśwież**. Po odświeżeniu listy kliknij nazwę magazynu.

### <a name="to-determine-storage-redundancy"></a>Aby określić nadmiarowość magazynu
Podczas tworzenia magazynu Usług odzyskiwania określany jest sposób replikowania magazynu.

1. Kliknij nowy magazyn, aby otworzyć pulpit nawigacyjny.
2. W bloku **Ustawienia**, otwieranym automatycznie wraz z pulpitem nawigacyjnym magazynu, kliknij pozycję **Infrastruktura kopii zapasowej**.
3. W bloku Infrastruktura kopii zapasowej kliknij pozycję **Konfiguracja kopii zapasowej**, aby wyświetlić **Typ replikacji magazynu**.
   
    ![Tworzenie magazynu Usług odzyskiwania — krok 5](./media/backup-try-azure-backup-in-10-mins/backup-infrastructure.png)
4. Wybierz odpowiednią opcję replikacji dla magazynu.
   
    ![Lista magazynów Usług odzyskiwania](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)
   
    Domyślnie magazyn jest geograficznie nadmiarowy. Jeśli używasz platformy Azure jako punktu końcowego podstawowego magazynu kopii zapasowych, kontynuuj korzystanie z magazynu geograficznie nadmiarowego. Jeśli używasz platformy Azure jako punktu końcowego magazynu kopii zapasowych innego niż podstawowy, wybierz magazyn lokalnie nadmiarowy, dzięki któremu zostanie obniżony koszt przechowywania danych na platformie Azure. Więcej informacji o opcjach magazynu [geograficznie nadmiarowego](../storage/storage-redundancy.md#geo-redundant-storage) i [lokalnie nadmiarowego](../storage/storage-redundancy.md#locally-redundant-storage) można znaleźć w tym [omówieniu](../storage/storage-redundancy.md).

Po utworzeniu magazynu należy przygotować infrastrukturę do utworzenia kopii zapasowej plików i folderów, pobierając poświadczenia magazynu i agenta Usług odzyskiwania Microsoft Azure.

## <a name="step-3---download-files"></a>Krok 3. Pobieranie plików
1. Kliknij pozycję **Ustawienia** na pulpicie nawigacyjnym magazynu Usług odzyskiwania.
   
    ![Otwieranie bloku celu kopii zapasowej](./media/backup-try-azure-backup-in-10-mins/settings-button.png)
2. Kliknij pozycję **Wprowadzenie > Kopia zapasowa** w bloku Ustawienia.
   
    ![Otwieranie bloku celu kopii zapasowej](./media/backup-try-azure-backup-in-10-mins/getting-started-backup.png)
3. Kliknij pozycję **Cel kopii zapasowej** w bloku Kopia zapasowa.
   
    ![Otwieranie bloku celu kopii zapasowej](./media/backup-try-azure-backup-in-10-mins/backup-goal.png)
4. Wybierz pozycję **Lokalnie** z menu „Gdzie działa Twoje obciążenie?”.
5. Wybierz pozycję **Pliki i foldery** z menu „Dla jakich elementów chcesz utworzyć kopię zapasową?”, a następnie kliknij przycisk **OK**.

### <a name="download-the-recovery-services-agent"></a>Pobieranie agenta Usług odzyskiwania
1. Kliknij pozycję **Pobierz agenta dla systemu Windows Server lub klienta systemu Windows** w bloku **Przygotowywanie infrastruktury**.
   
    ![Przygotowywanie infrastruktury](./media/backup-try-azure-backup-in-10-mins/prepare-infrastructure-short.png)
2. Kliknij pozycję **Zapisz** w oknie podręcznym pobierania. Domyślnie plik **MARSagentinstaller.exe** jest zapisywany w folderze Pobrane.

### <a name="download-vault-credentials"></a>Pobieranie poświadczeń magazynu
1. Kliknij pozycję **Pobierz > Zapisz** w bloku Przygotowywanie infrastruktury.
   
    ![Przygotowywanie infrastruktury](./media/backup-try-azure-backup-in-10-mins/prepare-infrastructure-download.png)

## <a name="step-4--install-and-register-the-agent"></a>Krok 4. Instalowanie i rejestrowanie agenta
> [!NOTE]
> Możliwość włączenia tworzenia kopii zapasowej za pośrednictwem Portalu Azure będzie dostępna wkrótce. Obecnie do tworzenia kopii zapasowej plików i folderów używany jest lokalny agent Usług odzyskiwania Microsoft Azure.
> 
> 

1. Zlokalizuj i kliknij dwukrotnie plik **MARSagentinstaller.exe** w folderze Pobrane (lub innej lokalizacji).
2. Wykonaj kroki kreatora instalacji agenta Usług odzyskiwania Microsoft Azure. Aby zakończyć pracę kreatora, wykonaj następujące czynności:
   
   * Wybierz lokalizację folderu instalacji i folderu pamięci podręcznej.
   * Podaj informacje o serwerze proxy, jeśli korzystasz z niego do łączenia się z Internetem.
   * Podaj swoją nazwę użytkownika i hasło, jeśli korzystasz z uwierzytelnionego serwera proxy.
   * Udostępnianie pobranych poświadczeń magazynu
   * Zapisz hasło szyfrowania w bezpiecznej lokalizacji.
     
     > [!NOTE]
     > Jeśli utracisz lub zapomnisz hasło, firma Microsoft nie będzie mogła pomóc w odzyskaniu danych kopii zapasowej. Hasło szyfrowania należy zapisać w bezpiecznej lokalizacji. Jest ono wymagane do przywrócenia kopii zapasowej.
     > 
     > 

Agent jest teraz zainstalowany, a maszyna zarejestrowana w magazynie. Wszystko jest gotowe do skonfigurowania kopii zapasowej i zaplanowania jej tworzenia.

## <a name="step-5-back-up-your-files-and-folders"></a>Krok 5: Tworzenie kopii zapasowej plików i folderów
Tworzenie początkowej kopii zapasowej obejmuje dwa kluczowe zadania:

* Planowanie tworzenia kopii zapasowej
* Tworzenie kopii zapasowej plików i folderów po raz pierwszy

Aby utworzyć początkową kopię zapasową, użyj agenta Usług odzyskiwania Microsoft Azure.

### <a name="to-schedule-the-backup"></a>Aby zaplanować tworzenie kopii zapasowej
1. Otwórz agenta Usług odzyskiwania Microsoft Azure. Aby go znaleźć, wyszukaj na maszynie łańcuch **Microsoft Azure Backup**.
   
    ![Uruchamianie agenta Usług odzyskiwania Microsoft Azure](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)
2. W agencie Usług odzyskiwania kliknij pozycję **Zaplanuj wykonywanie kopii zapasowej**.
   
    ![Planowanie tworzenia kopii zapasowej systemu Windows Server](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)
3. Na stronie Wprowadzenie Kreatora harmonogramu kopii zapasowej kliknij przycisk **Dalej**.
4. Na stronie Wybieranie elementów do wykonania kopii zapasowej kliknij pozycję **Dodaj elementy**.
5. Wybierz pliki i foldery, których kopię zapasową chcesz utworzyć, a następnie kliknij przycisk **OK**.
6. Kliknij przycisk **Dalej**.
7. Na stronie **Określanie harmonogramu tworzenia kopii zapasowych** określ **harmonogram tworzenia kopii zapasowej**, a następnie kliknij przycisk **Dalej**.
   
    Można zaplanować tworzenie kopii zapasowych codziennie (maksymalnie trzy razy) lub co tydzień.
   
    ![Elementy związane z tworzeniem kopii zapasowej systemu Windows Server](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)
   
   > [!NOTE]
   > Aby uzyskać więcej informacji o sposobie określania harmonogramu tworzenia kopii zapasowych, zobacz artykuł [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md) (Użycie usługi Azure Backup do zastąpienia infrastruktury taśm).
   > 
   > 
8. Na stronie **Wybieranie zasady przechowywania** wybierz pozycję **Zasady przechowywania** dla kopii zapasowej.
   
    Zasady przechowywania służą do określania czasu przechowywania kopii zapasowej. Zamiast określać wspólne zasady dla wszystkich punktów kopii zapasowej, można określić różne zasady przechowywania w zależności od momentu tworzenia kopii zapasowej. Możliwe jest modyfikowanie, zgodnie z potrzebami, zasad przechowywania codziennych, cotygodniowych, comiesięcznych i corocznych kopii zapasowych.
9. Na stronie Wybieranie typu początkowej kopii zapasowej wybierz typ początkowej kopii zapasowej. Pozostaw zaznaczoną opcję **Automatycznie przez sieć**, a następnie kliknij przycisk **Dalej**.
   
    Kopie zapasowe można tworzyć automatycznie za pośrednictwem sieci lub w trybie offline. W dalszej części tego artykułu opisano proces automatycznego tworzenia kopii zapasowych. Jeśli chcesz tworzyć kopie zapasowe w trybie offline, zapoznaj się z artykułem [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) (Przepływ pracy tworzenia kopii zapasowej w trybie offline w usłudze Azure Backup), aby uzyskać dodatkowe informacje.
10. Przejrzyj informacje na stronie Potwierdzenie, a następnie kliknij przycisk **Zakończ**.
11. Po ukończeniu harmonogramu tworzenia kopii zapasowej przez kreatora kliknij przycisk **Zamknij**.

### <a name="to-back-up-files-and-folders-for-the-first-time"></a>Aby utworzyć kopię zapasową plików i folderów po raz pierwszy
1. W agencie Usług odzyskiwania kliknij pozycję **Wykonaj kopię zapasową teraz**, aby zakończyć początkowe umieszczanie za pośrednictwem sieci.
   
    ![Natychmiastowe tworzenie kopii zapasowej systemu Windows Server](./media/backup-try-azure-backup-in-10-mins/backup-now.png)
2. Na stronie Potwierdzenie przejrzyj ustawienia, które zostaną użyte przez Kreatora natychmiastowego tworzenia kopii zapasowej do utworzenia kopii zapasowej maszyny. Następnie kliknij pozycję **Utwórz kopię zapasową**.
3. Kliknij przycisk **Zamknij**, aby zamknąć kreatora. Jeśli zrobisz to przed zakończeniem procesu tworzenia kopii zapasowej, kreator będzie nadal działał w tle.

Po zakończeniu tworzenia początkowej kopii zapasowej w konsoli usługi Backup zostanie wyświetlony stan **Ukończono zadanie**.

![Początkowa replikacja została zakończona](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a>Pytania?
Jeśli masz pytania lub jeśli brakuje Ci jakiejś funkcji, [prześlij nam opinię](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [tworzeniu kopii zapasowej maszyn z systemem Windows](backup-configure-vault.md).
* Teraz, gdy utworzono kopię zapasową plików i folderów, możesz [zarządzać swoimi magazynami i serwerami](backup-azure-manage-windows-server.md).
* Jeśli chcesz przywrócić kopię zapasową, w tym artykule znajdziesz informacje dotyczące [przywracania plików na maszynę z systemem Windows](backup-azure-restore-windows-server.md).




<!--HONumber=Dec16_HO1-->


