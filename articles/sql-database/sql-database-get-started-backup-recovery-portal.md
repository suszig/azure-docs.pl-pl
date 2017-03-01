---
title: 'Azure Portal: wykonywanie kopii zapasowej bazy danych Azure SQL Database i jej przywracanie | Microsoft Docs'
description: "W tym samouczku przedstawiono sposób przywracania automatycznych kopii zapasowych do punktu w czasie, przechowywania automatycznych kopii zapasowych w magazynie usługi Azure Recovery Services i przywracania z magazynu usługi Azure Recovery Services"
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 93efe1a08149e7c027830b03a9e426ac5a05b27b
ms.openlocfilehash: 444954fa2152fbd8b7e25b9787a13fa4a62ec77f


---
# <a name="tutorial-back-up-and-restore-an-azure-sql-database-using-the-azure-portal"></a>Samouczek: wykonywanie kopii zapasowej bazy danych Azure SQL Database i przywracanie jej przy użyciu witryny Azure Portal
Z tego samouczka nauczysz się używać witryny Azure Portal do wykonywania następujących czynności:

- Wyświetlanie istniejących kopii zapasowych bazy danych
- Przywracanie bazy danych do określonego punktu w czasie
- Konfigurowanie długoterminowego przechowywania pliku kopii zapasowej bazy danych w magazynie usługi Azure Recovery Services
- Przywracanie bazy danych z magazynu usługi Azure Recovery Services

**Szacowany czas**: ukończenie tego samouczka zajmuje około 30 minut (przy założeniu, że spełniasz już wymagania wstępne).

> [!TIP]
> Te same czynności we wprowadzającym samouczku można wykonać przy użyciu programu [PowerShell](sql-database-get-started-backup-recovery-powershell.md).
>

## <a name="prerequisites"></a>Wymagania wstępne

* Musisz mieć konto platformy Azure. Możesz [utworzyć konto bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* Musisz być w stanie połączyć się z witryną Azure Portal przy użyciu konta, które jest członkiem roli właściciela subskrypcji albo współautora. Aby uzyskać więcej informacji o kontroli dostępu na podstawie ról (RBAC, role-based access control), zobacz [Getting started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md) (Wprowadzenie do zarządzania dostępem w witrynie Azure Portal).

* Ukończono kroki samouczka [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu witryny Azure Portal i programu SQL Server Management Studio](sql-database-get-started.md) lub samouczka w odpowiedniej [wersji dla programu PowerShell](sql-database-get-started-powershell.md). W przeciwnym razie przed kontynuowaniem wykonaj kroki tego samouczka dotyczącego wymagań wstępnych lub uruchom skrypt PowerShell pod koniec pracy z tym samouczkiem w [wersji dla programu PowerShell](sql-database-get-started-powershell.md).


> [!NOTE]
> Ten samouczek ułatwia opanowanie informacji podanych w następujących tematach szkoleniowych: [SQL Database backups](sql-database-automated-backups.md) (Kopie zapasowe usługi SQL Database), [Long-term backup retention](sql-database-long-term-retention.md) (Długoterminowe przechowywanie kopii zapasowych) oraz [Recover an Azure SQL database using automated database backups](sql-database-recovery-using-backups.md) (Odzyskiwanie bazy danych Azure SQL przy użyciu zautomatyzowanych kopii zapasowych baz danych).
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Logowanie do witryny Azure Portal przy użyciu konta platformy Azure
Za pomocą [istniejącej subskrypcji](https://account.windowsazure.com/Home/Index) wykonaj poniższe kroki, aby połączyć się z witryną Azure Portal.

1. Otwórz wybraną przeglądarkę i połącz się z witryną [Azure Portal](https://portal.azure.com/).
2. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
3. Kiedy zostanie wyświetlona strona **logowania**, podaj poświadczenia subskrypcji.
   
   ![Logowanie](./media/sql-database-get-started/login.png)

<a name="create-logical-server-bk"></a>

## <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database"></a>Wyświetlanie najstarszego punktu przywracania z kopii zapasowych bazy danych wygenerowanych przez usługę

W tej części samouczka zapoznasz się z informacjami o najstarszym punkcie przywracania związanym z [wygenerowanymi przez usługi automatycznymi kopiami zapasowymi](sql-database-automated-backups.md) bazy danych. 

1. Otwórz blok **Baza danych SQL** dla bazy danych **sqldbtutorialdb**.

   ![blok nowej przykładowej bazy danych](./media/sql-database-get-started/new-sample-db-blade.png)

2. Na pasku narzędzi kliknij pozycję **Przywróć**.

   ![pasek narzędzi przywracania](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

3. W bloku przywracania przejrzyj najstarszy punkt przywracania.

   ![najstarszy punkt przywracania](./media/sql-database-get-started-backup-recovery/oldest-restore-point.png)

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Przywracanie bazy danych do określonego punktu w czasie

W tej części samouczka przywrócisz bazę danych do nowej bazy danych z określonego punktu w czasie.

1. W bloku **Przywracanie** bazy danych sprawdź domyślną nazwę nowej bazy danych, do której chcesz przywrócić bazę danych z wcześniejszego punktu w czasie (jest to nazwa istniejącej bazy danych z dołączoną sygnaturą czasową). Zmiany tej nazwy odzwierciedlają czas określony w kolejnych krokach.

   ![nazwa przywracanej bazy danych](./media/sql-database-get-started-backup-recovery/restored-database-name.png)

2. Kliknij ikonę **kalendarza** w polu wejściowym **Punkt przywracania (UTC)**.

   ![punkt przywracania](./media/sql-database-get-started-backup-recovery/restore-point.png)

2. W kalendarzu wybierz datę w okresie przechowywania.

   ![data punktu przywracania](./media/sql-database-get-started-backup-recovery/restore-point-date.png)

3. W polu wejściowym **Punkt przywracania (UTC)** określ godzinę w wybranym dniu, do której chcesz przywrócić dane w bazie danych z automatycznych kopii zapasowych bazy danych.

   ![godzina punktu przywracania](./media/sql-database-get-started-backup-recovery/restore-point-time.png)

   >[!NOTE]
   >Zauważ, że nazwa bazy danych zmieniła się, aby odzwierciedlić wybraną datę i godzinę. Zauważ również, że nie możesz zmienić serwera, na którym odbywa się przywracanie do określonego punktu w czasie. Aby przywrócić na innym serwerze, należy użyć funkcji [przywracania geograficznego](sql-database-disaster-recovery.md#recover-using-geo-restore). Na koniec zauważ, że można przywrócić do [puli elastycznej](sql-database-elastic-jobs-overview.md) lub do innej warstwy cenowej. 
   >

4. Kliknij przycisk **OK**, aby przywrócić bazę danych do wcześniejszego punktu w czasie dla nowej bazy danych.

5. Na pasku narzędzi kliknij ikonę powiadomienia, aby wyświetlić stan zadania przywracania.

   ![postęp zadania przywracania](./media/sql-database-get-started-backup-recovery/restore-job-progress.png)

6. Po zakończeniu zadania przywracania otwórz blok **Bazy danych SQL**, aby wyświetlić nowo przywróconą bazę danych.

   ![przywrócona baza danych](./media/sql-database-get-started-backup-recovery/restored-database.png)

> [!NOTE]
> W tym miejscu możesz połączyć się z przywróconą bazą danych przy użyciu programu SQL Server Management Studio, aby wykonać niezbędne zadania, takie jak [wyodrębnienie bitu danych z przywróconej bazy danych w celu skopiowania do istniejącej bazy danych lub usunięcie istniejącej bazę danych i zmiana nazwy przywróconej bazy danych na istniejącą nazwę bazy danych](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Konfigurowanie długoterminowego przechowywania automatycznych kopii zapasowych w magazynie usługi Azure Recovery Services 

W tej części samouczka możesz [skonfigurować magazyn usługi Azure Recovery Services do przechowywania automatycznych kopii zapasowych](sql-database-long-term-retention.md) przez okres dłuższy niż okres przechowywania w warstwie usługi. 


> [!TIP]
> Aby usunąć kopie zapasowe objęte długoterminowym przechowywaniem kopii zapasowych, zobacz [Manage long-term backup retention using PowerShell](sql-database-manage-long-term-backup-retention-powershell.md) (Zarządzanie długotrwałym przechowywaniem kopii zapasowych za pomocą programu PowerShell).
>

1. Otwórz blok **SQL Server** dla serwera **sqldbtutorialserver**.

   ![blok serwera SQL](./media/sql-database-get-started/sql-server-blade.png)

2. Kliknij pozycję **Długoterminowe przechowywanie kopii zapasowych**.

   ![link długoterminowego przechowywania kopii zapasowych](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. W bloku **sqldbtutorial — długoterminowe przechowywani kopii zapasowych** przejrzyj i zaakceptuj warunki dotyczące wersji zapoznawczej (chyba że ta czynność została już wykonana lub ta funkcja nie jest już dostępna w wersji zapoznawczej).

   ![akceptowanie warunków wersji zapoznawczej](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Aby skonfigurować długoterminowe przechowywanie kopii zapasowych bazy danych sqldbtutorialdb, wybierz tę bazę danych w siatce, a następnie kliknij pozycję **Konfiguruj** na pasku narzędzi.

   ![wybieranie bazy danych do długoterminowego przechowywania kopii zapasowych](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. W bloku **Konfigurowanie** kliknij pozycję **Skonfiguruj wymagane ustawienia** w obszarze **Magazyn usługi Recovery Services**.

   ![konfigurowanie linku do magazynu](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. W bloku **Magazyn usługi Recovery Services** wybierz magazyn, jeśli istnieje. W przeciwnym razie, jeśli nie można odnaleźć magazynu usługi Recovery Services powiązanego z subskrypcją, kliknij, aby zakończyć przepływ i utworzyć magazyn usługi Recovery Services.

   ![tworzenie nowego linku do magazynu](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. W bloku **Magazyny usługi Recovery Services** kliknij pozycję **Dodaj**.

   ![dodawanie nowego linku do magazynu](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. W bloku **Magazyn usługi Recovery Services** podaj prawidłową nazwę nowego magazynu usługi Recovery Services.

   ![nazwa nowego magazynu](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Wybierz subskrypcję i grupę zasobów, a następnie wybierz lokalizację magazynu. Gdy skończysz, kliknij pozycję **Utwórz**.

   ![tworzenie nowego magazynu](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > Magazyn musi znajdować się w tym samym regionie co serwer logiczny Azure SQL i musi używać tej samej grupy zasobów co serwer logiczny.
   >

10. Po utworzeniu nowego magazynu wykonaj niezbędne kroki, aby wrócić do bloku **Magazyn usługi Recovery Services**.

11. W bloku **Magazyn usługi Recovery Services** kliknij magazyn, a następnie kliknij pozycję **Wybierz**.

   ![wybieranie istniejącego magazynu](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. W bloku **Konfigurowanie** podaj prawidłową nazwę nowych zasad przechowywania, zmodyfikuj odpowiednio domyślne zasady przechowywania, a następnie kliknij przycisk **OK**.

   ![definiowanie zasad przechowywania](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. W bloku **sqldbtutorial — długoterminowe przechowywanie kopii zapasowych** kliknij pozycję **Zapisz**, a następnie kliknij przycisk **OK**, aby zastosować zasady długoterminowego przechowywania kopii zapasowych do wszystkich wybranych baz danych.

   ![definiowanie zasad przechowywania](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Kliknij pozycję **Zapisz**, aby włączyć opcję długoterminowego przechowywania kopii zapasowych przy użyciu nowych zasad w skonfigurowanym magazynie usługi Azure Recovery Services.

   ![definiowanie zasad przechowywania](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

15. Po włączeniu długoterminowego przechowywania kopii zapasowych otwórz blok **sqldbtutorialvault** (przejdź do pozycji **Wszystkie zasoby** i wybierz go z listy zasobów dla subskrypcji).

   ![wyświetlanie magazynu usługi recovery services](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault.png)

> [!IMPORTANT]
> Skonfigurowane kopie zapasowe zostaną wyświetlone w magazynie w ciągu następnych siedmiu dni. Pracę z tym samouczkiem można kontynuować po wyświetleniu kopii zapasowych w magazynie.
>

## <a name="view-backups-in-long-term-retention"></a>Wyświetlanie kopii zapasowych podlegających długoterminowemu przechowywaniu

W tej części samouczka wyświetlisz informacje dotyczące kopii zapasowych bazy danych podlegających [długoterminowemu przechowywaniu kopii zapasowych](sql-database-long-term-retention.md). 

1. Otwórz blok **sqldbtutorialvault** (przejdź do pozycji **Wszystkie zasoby** i wybierz go z listy zasobów dla subskrypcji), aby wyświetlić rozmiar magazynu używany przez kopie zapasowe bazy danych.

   ![wyświetlanie magazynu usługi recovery services z kopiami zapasowymi](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Otwórz blok **Baza danych SQL** dla bazy danych **sqldbtutorialdb**.

   ![blok nowej przykładowej bazy danych](./media/sql-database-get-started/new-sample-db-blade.png)

3. Na pasku narzędzi kliknij pozycję **Przywróć**.

   ![pasek narzędzi przywracania](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. W bloku Przywracanie kliknij pozycję **Długoterminowe**.

5. W obszarze kopii zapasowych magazynu platformy Azure kliknij pozycję **Wybierz kopię zapasową**, aby wyświetlić dostępne kopie zapasowych bazy danych podlegające długoterminowemu przechowywaniu kopii zapasowych.

   ![kopie zapasowe w magazynie](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

## <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Przywracanie bazy danych z kopii zapasowej podlegającej długoterminowemu przechowywaniu kopii zapasowych

W tej części samouczka przywrócisz bazę danych do nowej bazy danych z kopii zapasowej w magazynie usługi Azure Recovery Services.

1. W bloku **kopii zapasowych w magazynie platformy Azure** kliknij kopię zapasową do przywrócenia, a następnie kliknij pozycję **Wybierz**.

   ![wybieranie kopii zapasowej w magazynie](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. W polu tekstowym **Nazwa bazy danych** podaj nazwę przywracanej bazy danych.

   ![nazwa nowej nazwy danych](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Kliknij przycisk **OK**, aby przywrócić bazę danych z kopii zapasowej w magazynie do nowej bazy danych.

4. Na pasku narzędzi kliknij ikonę powiadomienia, aby wyświetlić stan zadania przywracania.

   ![postęp zadania przywracania z magazynu](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Po zakończeniu zadania przywracania otwórz blok **Bazy danych SQL**, aby wyświetlić nowo przywróconą bazę danych.

   ![baza danych przywrócona z magazynu](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> W tym miejscu możesz połączyć się z przywróconą bazą danych przy użyciu programu SQL Server Management Studio, aby wykonać niezbędne zadania, takie jak [wyodrębnienie bitu danych z przywróconej bazy danych w celu skopiowania do istniejącej bazy danych lub usunięcie istniejącej bazę danych i zmiana nazwy przywróconej bazy danych na istniejącą nazwę bazy danych](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej o automatycznych kopiach zapasowych generowanych przez usługi, zobacz artykuł dotyczący [automatycznych kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się więcej o długoterminowym przechowywaniu kopii zapasowych, zobacz temat dotyczący [długoterminowego przechowywania kopii zapasowych](sql-database-long-term-retention.md)
- Aby dowiedzieć się więcej o przywracaniu z kopii zapasowych, zobacz temat dotyczący [przywracania z kopii zapasowej](sql-database-recovery-using-backups.md)



<!--HONumber=Feb17_HO3-->


