---
title: 'Szybki start: pierwsza baza danych SQL na platformie Azure | Microsoft Docs'
description: "Dowiedz się, jak utworzyć serwer logiczny, regułę zapory na poziomie serwera i bazy danych usługi SQL Database w witrynie Azure Portal. Dowiedz się także, jak używać programu SQL Server Management Studio z usługą Azure SQL Database."
keywords: "samouczek usługi sql database, tworzenie bazy danych sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: single databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 166a9d7032bb75188a790bea1724aefd194dcefa
ms.openlocfilehash: 36afd5c8bccb080ae3aaf1b4975d317b9087a3b3


---
# <a name="create-connect-to-and-query-your-first-azure-sql-databases-in-the-azure-portal-and-using-ssms"></a>Tworzenie pierwszych baz danych Azure SQL Database, łączenie się z nimi i kierowanie do nich zapytań w witrynie Azure Portal i za pomocą programu SSMS

Z tego samouczka nauczysz się, jak tworzyć bazy danych Azure SQL Database, łączyć się z nimi i kierować do nich zapytania w witrynie Azure Portal i przy użyciu programu SQL Server Management Studio. Po ukończeniu tego samouczka:

* Utworzysz grupę zasobów zawierającą serwer logiczny, regułę zapory na poziomie serwera i dwie bazy danych.
* Dowiesz się, jak wyświetlić właściwości serwera i bazy danych w witrynie Azure Portal i za pomocą programu SQL Server Management Studio.
* Dowiesz się, jak skierować zapytanie do bazy danych w witrynie Azure Portal i za pomocą programu SQL Server Management Studio.

**Szacowany czas**: ten samouczek zajmuje około 30 minut (przy założeniu, że spełniasz już wymagania wstępne).

> [!TIP]
> Możesz też dowiedzieć się, jak tworzyć bazy danych Azure SQL Database, łączyć się z nimi i kierować do nich zapytania, używając programu [PowerShell](sql-database-get-started-powershell.md) lub języka [C#](sql-database-get-started-csharp.md).
>

> [!NOTE]
> Ten samouczek ułatwia opanowanie informacji podanych w następujących tematach szkoleniowych: [SQL Database server overview](sql-database-server-overview.md) (Omówienie serwerów usługi SQL Database), [Omówienie usługi Azure SQL Database](sql-database-overview.md) i [Omówienie reguł zapory usługi Azure SQL Database](sql-database-firewall-configure.md). Aby zapoznać się z przeglądem usługi SQL Database, zobacz [Co to jest SQL Database?](sql-database-technical-overview.md).
>  

## <a name="prerequisites"></a>Wymagania wstępne

* **Konto platformy Azure**. Możesz [utworzyć konto bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Uprawnienia do tworzenia na platformie Azure**. Musisz być w stanie połączyć się z witryną Azure Portal przy użyciu konta, które jest członkiem roli właściciela subskrypcji albo współautora. Aby uzyskać więcej informacji o kontroli dostępu na podstawie ról (RBAC, role-based access control), zobacz [Getting started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md) (Wprowadzenie do zarządzania dostępem w witrynie Azure Portal).

* **SQL Server Management Studio**. Najnowszą wersję programu SSMS możesz zainstalować po pobraniu ze strony [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (Pobieranie programu SQL Server Management Studio). Zawsze używaj najnowszej wersji programu SSMS podczas łączenia się z usługą Azure SQL Database, ponieważ cały czas wydawane są nowe funkcje.

### <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Kroki opisane w tej procedurze pokazują, jak łączyć się z witryną Azure Portal za pomocą [konta platformy Azure](https://account.windowsazure.com/Home/Index).

1. Otwórz wybraną przeglądarkę i połącz się z witryną [Azure Portal](https://portal.azure.com/).
2. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
3. Kiedy zostanie wyświetlona strona **logowania**, podaj poświadczenia subskrypcji.
   
   ![Logowanie](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server"></a>Tworzenie nowego serwera logicznego SQL

Kroki opisane w tej procedurze pokazują, jak w witrynie Azure Portal utworzyć serwer logiczny w wybranym przez siebie regionie. Serwer logiczny to obiekt, w którym tworzone są bazy danych SQL i reguły zapory zezwalające użytkownikom na łączenie się przez zaporę usługi Azure SQL Database. 

1. Kliknij pozycję **Nowy**, wpisz ciąg **sql server**, a następnie naciśnij klawisz **ENTER**.

    ![serwer logiczny SQL](./media/sql-database-get-started/logical-sql-server.png)
2. Kliknij pozycję **SQL Server (serwer logiczny)**.
   
    ![tworzenie serwera logicznego SQL](./media/sql-database-get-started/create-logical-sql-server.png)
3. Kliknij pozycję **Utwórz**, aby otworzyć nowy blok SQL Server (tylko serwer logiczny).

    ![nowy serwer logiczny SQL](./media/sql-database-get-started/new-logical-sql-server.png)
3. W polu tekstowym **Nazwa serwera** podaj prawidłową nazwę nowego serwera logicznego. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
    
    ![nazwa nowego serwera](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > W pełni kwalifikowana nazwa nowego serwera musi być globalnie unikatowa i ma postać **<Twoja_nazwa_serwera>.database.windows.net**. Ta w pełni kwalifikowana nazwa serwera będzie używana później w trakcie tego samouczka do łączenia się z serwerem i bazami danych.
    >
    
4. W polu tekstowym **Nazwa logowania administratora serwera** podaj nazwę użytkownika na potrzeby logowania w celu uwierzytelniania SQL dla tego serwera. Ta nazwa logowania jest nazywana główną nazwą logowania serwera. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
    
    ![identyfikator logowania administratora SQL](./media/sql-database-get-started/sql-admin-login.png)
5. W polach tekstowych **Hasło** i **Potwierdź hasło** podaj hasło do głównego konta logowania serwera. Zielony znacznik wyboru wskazuje, czy podane hasło jest poprawne.
    
    ![Hasło administratora SQL](./media/sql-database-get-started/sql-admin-password.png)
6. W polu rozwijanym **Subskrypcja** wybierz subskrypcję, w której masz uprawnienia do tworzenia obiektów.

    ![subskrypcja](./media/sql-database-get-started/subscription.png)
7. W polu tekstowym**Grupa zasobów** wybierz pozycję **Wybierz nowy**, a następnie podaj prawidłową nazwę dla nowej grupy zasobów. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.

    ![nowa grupa zasobów](./media/sql-database-get-started/new-resource-group.png)

8. W polu tekstowym **Lokalizacja** wybierz centrum danych, w którym ma zostać utworzony serwer logiczny.
    
    ![lokalizacja serwera](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > W tym bloku nie można zmienić pola wyboru dla opcji **Zezwalaj usługom platformy Azure na dostęp do serwera**. To ustawienie można zmienić w bloku zapory serwera. Aby uzyskać więcej informacji, zobacz artykuł [Wprowadzenie do zabezpieczeń](sql-database-control-access-sql-authentication-get-started.md).
    >
    
9. Zaznacz pole wyboru **Przypnij do pulpitu nawigacyjnego**.

10. Kliknij pozycję **Utwórz**, aby wdrożyć ten skrypt na platformie Azure w celu utworzenia serwera logicznego.

    ![przycisk Utwórz](./media/sql-database-get-started/create.png)

11. Po utworzeniu serwera przejrzyj jego właściwości, które są wyświetlane domyślnie. 

    ![blok serwera SQL](./media/sql-database-get-started/sql-server-blade.png)
12. Kliknij pozycję **Właściwości**, aby wyświetlić dodatkowe właściwości serwera logicznego SQL.

    ![właściwości serwera SQL](./media/sql-database-get-started/sql-server-properties.png)
13. Skopiuj w pełni kwalifikowaną nazwę serwera do schowka, aby skorzystać z niej na dalszym etapie tego samouczka.

    ![pełna nazwa serwera SQL](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule"></a>Tworzenie reguły zapory na poziomie serwera

Kroki opisane w tej procedurze pokazują, jak utworzyć regułę zapory na poziomie serwera w witrynie Azure Portal. Domyślnie zapora usługi Azure SQL Database uniemożliwia połączenia z zewnątrz z Twoim serwerem logicznym i jego bazami danych. Aby umożliwić łączenie z serwerem, musisz utworzyć regułę zapory dla adresu IP komputera, z którego się łączysz w następnej procedurze. Aby uzyskać więcej informacji, zobacz [Omówienie reguł zapory usługi Azure SQL Database](sql-database-firewall-configure.md).

1. W bloku serwera SQL kliknij pozycję **Zapora**, aby otworzyć blok Zapora dla Twojego serwera. Zwróć uwagę, że wyświetlany jest adres IP Twojego komputera klienckiego.

    ![zapora serwera SQL](./media/sql-database-get-started/sql-server-firewall.png)

2. Kliknij pozycję **Dodaj adres IP klienta** na pasku narzędzi, aby utworzyć regułę zapory dla bieżącego adresu IP.

    ![dodawanie adresu IP klienta](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Regułę zapory można utworzyć dla pojedynczego adresu IP lub całego zakresu adresów. Otwarcie zapory pozwala administratorom i użytkownikom usługi SQL logować się do każdej bazy danych na serwerze, do którego mają ważne poświadczenia.
    >

4. Kliknij pozycję **Zapisz** na pasku narzędzi, aby zapisać tę regułę zapory na poziomie serwera, a następnie kliknij przycisk **OK**, aby zamknąć okno dialogowe informujące o powodzeniu.

    ![powodzenie](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-the-server-with-ssms"></a>Nawiązywanie połączenia z serwerem za pomocą programu SSMS

Kroki opisane w tej procedurze pokazują, jak nawiązać połączenie z serwerem logicznym SQL za pomocą programu SQL Server Management Studio. Program SSMS to podstawowe narzędzie używane przez administratorów baz danych do zarządzania serwerami i bazami danych SQL.

1. Otwórz program SQL Server Management Studio (w polu wyszukiwania systemu Windows wpisz nazwę **Microsoft SQL Server Management Studio**, a następnie naciśnij klawisz **Enter**, aby otworzyć program SSMS).

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. W oknie dialogowym **Łączenie z serwerem** wprowadź w pełni kwalifikowaną nazwę serwera z poprzedniej procedury, wybierz pozycję Uwierzytelnianie SQL Server, a następnie podaj nazwę logowania i hasło określone podczas aprowizowania serwera.

    ![łączenie z serwerem](./media/sql-database-get-started/connect-to-server.png)
4. Kliknij pozycję **Połącz**, aby zainicjować połączenie i otworzyć Eksploratora obiektów w programie SSMS.

    ![nawiązane połączenie z serwerem](./media/sql-database-get-started/connected-to-server.png)
5. W Eksploratorze obiektów rozwiń kolejno węzły **Bazy danych**, **Systemowe bazy danych** i **master**, aby wyświetlić obiekty w bazie danych master.

    ![baza danych master](./media/sql-database-get-started/master-database.png)
6. Kliknij prawym przyciskiem myszy pozycję **master**, a następnie kliknij pozycję **Nowe zapytanie**.

    ![odpytywanie bazy danych master](./media/sql-database-get-started/query-master-database.png)

8. W oknie zapytania wpisz następujące zapytanie:

   ```select * from sys.objects```

9.  Na pasku narzędzi kliknij przycisk **Wykonaj**, aby zwrócić listę wszystkich obiektów systemowych w bazie danych master.

    ![odpytywanie bazy danych master o obiekty systemowe](./media/sql-database-get-started/query-master-database-system-objects.png)

    > [!NOTE]
    > Aby rozpocząć pracę z zabezpieczeniami SQL, zobacz [Get Started with SQL authentication](sql-database-control-access-sql-authentication-get-started.md) (Wprowadzenie do uwierzytelniania SQL)
    >

## <a name="create-a-database-with-sample-data"></a>Tworzenie bazy danych z przykładowymi danymi

Kroki opisane w tej procedurze pokazują, jak w witrynie Azure Portal utworzyć bazę danych z przykładowymi danymi skojarzoną z wcześniej utworzonym serwerem logicznym. 

1. W witrynie Azure Portal kliknij pozycję **Bazy danych SQL** w bloku domyślnym.

    ![Bazy danych SQL](./media/sql-database-get-started/new-sql-database.png)
2. W bloku Bazy danych SQL kliknij pozycję **Dodaj**. 

    ![dodawanie bazy danych SQL](./media/sql-database-get-started/add-sql-database.png)

    ![blok Baza danych SQL](./media/sql-database-get-started/sql-database-blade.png)
3. W polu tekstowym **Nazwa bazy danych** podaj prawidłową nazwę bazy danych.

    ![nazwa bazy danych SQL](./media/sql-database-get-started/sql-database-name.png)
4. W obszarze **Wybierz źródło** wybierz pozycję **Przykład (AdventureWorksLT)**.
   
    ![Adventure Works LT](./media/sql-database-get-started/adventureworkslt.png)
5. W obszarze **Serwer** sprawdź, czy Twój serwer jest wybrany. Zwróć uwagę, że bazę danych można dodać do serwera jako pojedynczą bazę danych (jest to ustawienie domyślne) lub dodać ją do elastycznej puli. Aby uzyskać więcej informacji o pulach elastycznych, zobacz artykuł [Pule elastyczne](sql-database-elastic-pool.md).

6. W obszarze **Warstwa cenowa** zmień warstwę cenową na **Podstawowa** i kliknij pozycję **Wybierz**. W razie potrzeby warstwę cenową można później zwiększyć, ale dla celów szkoleniowych zalecamy użycie najtańszej warstwy.

    ![warstwa cenowa](./media/sql-database-get-started/pricing-tier.png)
7. Zaznacz pole wyboru **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij pozycję **Utwórz**.

    ![przycisk Utwórz](./media/sql-database-get-started/create.png)

8. Po utworzeniu bazy danych obejrzyj jej właściwości w witrynie Azure Portal. Kolejne samouczki pomogą Ci zrozumieć opcje dostępne w tym bloku. 

    ![blok nowej przykładowej bazy danych](./media/sql-database-get-started/new-sample-db-blade.png)

## <a name="query-the-database-in-the-azure-portal"></a>Tworzenie zapytań względem bazy danych w witrynie Azure Portal

Kroki opisane w tej procedurze pokazują, jak kierować zapytania do bazy danych bezpośrednio z witryny Azure Portal. 

1. W bloku Bazy danych SQL kliknij pozycję **Narzędzia** na pasku narzędzi.

    ![narzędzia](./media/sql-database-get-started/tools.png)
2. W bloku Narzędzia kliknij pozycję **Edytor zapytań (wersja zapoznawcza)**.

    ![edytor zapytań](./media/sql-database-get-started/query-editor.png)
3. Kliknij pole wyboru, aby potwierdzić, że edytor zapytań jest funkcją w wersji zapoznawczej, a następnie kliknij przycisk **OK**.
4. W bloku **Edytor zapytań** kliknij pozycję **Zaloguj się**.

    ![blok edytora zapytań](./media/sql-database-get-started/query-editor-blade.png)
5. Przejrzyj pozycje Typ autoryzacji i Konto logowania, a następnie podaj hasło dla tego konta logowania. 

    ![logowanie w edytorze zapytań](./media/sql-database-get-started/query-editor-login.png)
6. Kliknij przycisk **OK**, aby spróbować się zalogować.
7. Po uwierzytelnieniu w oknie zapytania wpisz następujące zapytanie, a następnie kliknij pozycję **Uruchom**.

   ```select * from sys.objects```

    ![zapytanie edytora zapytań](./media/sql-database-get-started/query-editor-query.png)

8. Przejrzyj wyniki zapytania w okienku **Wyniki**.

    ![wyniki edytora zapytań](./media/sql-database-get-started/query-editor-results.png)

## <a name="query-the-database-with-ssms"></a>Kierowanie zapytań do bazy danych za pomocą programu SSMS

Kroki opisane w tej procedurze pokazują, jak nawiązać połączenie z bazą danych za pomocą programu SQL Server Management Studio, a następnie utworzyć zapytanie względem przykładowych danych w celu wyświetlenia obiektów w bazie danych.

1. Przełącz się do programu SQL Server Management Studio i w Eksploratorze obiektów kliknij pozycję **Bazy danych**, a następnie przycisk **Odśwież** na pasku narzędzi, aby wyświetlić przykładową bazę danych.

    ![nowa przykładowa baza danych w programie SSMS](./media/sql-database-get-started/new-sample-db-ssms.png)
2. W Eksploratorze obiektów rozwiń węzeł nowej bazy danych, aby wyświetlić jej obiekty.

    ![obiekty nowej przykładowej bazy danych w programie SSMS](./media/sql-database-get-started/new-sample-db-objects-ssms.png)
3. Kliknij prawym przyciskiem myszy przykładową bazę danych, a następnie kliknij pozycję **Nowe zapytanie**.

    ![zapytanie względem nowej przykładowej bazy danych w programie SSMS](./media/sql-database-get-started/new-sample-db-query-ssms.png)
4. W oknie zapytania wpisz następujące zapytanie:

   ```select * from sys.objects```
   
9.  Na pasku narzędzi kliknij przycisk **Wykonaj**, aby zwrócić listę wszystkich obiektów systemowych w przykładowej bazie danych.

    ![zapytanie względem nowej przykładowej bazy danych o obiekty systemowe w programie SSMS](./media/sql-database-get-started/new-sample-db-query-objects-ssms.png)

## <a name="create-a-blank-database-with-ssms"></a>Tworzenie pustej bazy danych za pomocą programu SSMS

Kroki opisane w tej procedurze pokazują, jak utworzyć nową bazę danych za pomocą programu SQL Server Management Studio.

1. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **Bazy danych**, a następnie kliknij pozycję **Nowa baza danych**.

    ![nowa pusta baza danych w programie SSMS](./media/sql-database-get-started/new-blank-database-ssms.png)

2. W oknie dialogowym **Nowa baza danych** podaj nazwę bazy danych w polu tekstowym Nazwa bazy danych. 

    ![nazwa nowej pustej bazy danych w programie SSMS](./media/sql-database-get-started/new-blank-database-name-ssms.png)

3. W oknie dialogowym Nowa baza danych kliknij pozycję **Opcje**, a następnie zmień wersję na wartość **Podstawowa**.

    ![opcje nowej pustej bazy danych w programie SSMS](./media/sql-database-get-started/new-blank-database-options-ssms.png)

    > [!TIP]
    > Zapoznaj się z innymi opcjami w tym oknie dialogowym, które można zmodyfikować dla bazy danych Azure SQL Database. Aby uzyskać więcej informacji o tych opcjach, zobacz artykuł [Create Database](https://msdn.microsoft.com/library/dn268335.aspx) (Instrukcja CREATE DATABASE).
    >

4. Kliknij przycisk **OK**, aby utworzyć pustą bazę danych.
5. Po zakończeniu odśwież węzeł Baza danych w Eksploratorze obiektów, aby wyświetlić nowo utworzoną pustą bazę danych. 

    ![nowa pusta baza danych w Eksploratorze obiektów](./media/sql-database-get-started/new-blank-database-object-explorer.png)

## <a name="troubleshoot-connectivity"></a>Rozwiązywanie problemów z łącznością

Gdy nawiązywanie połączenia z usługą Azure SQL Database nie powiedzie się, pojawią się komunikaty o błędach. Problemy z połączeniami mogą być spowodowane ponownym skonfigurowaniem bazy danych SQL Azure, ustawieniami zapory, przekroczeniem limitu czasu połączenia lub nieprawidłowymi informacjami logowania. Aby uzyskać narzędzie do rozwiązywania problemów z połączeniami, zobacz [Troubleshooting connectivity issues with Microsoft Azure SQL Database](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) (Rozwiązywanie problemów z połączeniami z usługą Microsoft Azure SQL Database).

## <a name="delete-a-single-database-in-the-azure-portal"></a>Usuwanie pojedynczej bazy danych w witrynie Azure Portal

Kroki opisane w tej procedurze pokazują, jak za pomocą witryny Azure Portal usunąć pojedynczą bazę danych.

1. W bloku baz danych SQL w witrynie Azure Portal kliknij bazę danych, którą chcesz usunąć. 
2.  dla bazy danych SQL i kliknij przycisk **Usuń**.

    ![usuwanie bazy danych](./media/sql-database-get-started/delete-database.png)
2. Kliknij przycisk **Tak**, aby potwierdzić, że chcesz trwale usunąć tę bazę danych.

    ![potwierdzanie usunięcia bazy danych](./media/sql-database-get-started/delete-database-yes.png)

> [!TIP]
> Podczas okresu przechowywania bazy danych możesz ją przywrócić z automatycznych kopii zapasowych zainicjowanych przez usługę (o ile nie zostanie usunięty sam serwer). Bazy danych w wersji Podstawowa możesz przywrócić w ciągu siedmiu dni. W przypadku wszystkich innych wersji przywracanie jest możliwe w ciągu 35 dni. Jeśli usunięty zostanie sam serwer, nie będzie można odzyskać ani serwera, ani żadnej z jego usuniętych baz danych. Aby uzyskać więcej informacji o kopiach zapasowych bazy danych, zobacz [Dowiedz się więcej o kopiach zapasowych w usłudze SQL Database](sql-database-automated-backups.md). Aby uzyskać więcej informacji o przywracaniu bazy danych z kopii zapasowych, zobacz [Odzyskiwanie bazy danych](sql-database-recovery-using-backups.md). Aby uzyskać instrukcje dotyczące przywracania usuniętej bazy danych, zobacz [Przywracanie usuniętej bazy danych SQL na platformie Azure — Azure Portal](sql-database-restore-deleted-database-portal.md).
>


## <a name="next-steps"></a>Następne kroki
Po ukończeniu tego samouczka możesz zapoznać się z kilkoma dodatkowymi samouczkami, które poszerzą zdobytą wiedzę. 

- Samouczek z wprowadzeniem do uwierzytelniania programu SQL Server można znaleźć na stronie [Uwierzytelnianie i autoryzacja SQL](sql-database-control-access-sql-authentication-get-started.md)
- Samouczek z wprowadzeniem do uwierzytelniania usługi Azure Active Directory można znaleźć na stronie [Uwierzytelnianie i autoryzacja usługi AAD](sql-database-control-access-aad-authentication-get-started.md)
* Jeśli chcesz wykonać zapytanie w przykładowej bazie danych w witrynie Azure Portal, zobacz [Publiczna wersja zapoznawcza: zapytania interakcyjne dla baz danych SQL](https://azure.microsoft.com/updates/azure-sql-database-public-preview-t-sql-editor/)
* Jeśli posługujesz się programem Excel, naucz się [nawiązywać połączenie z bazą danych SQL w programie Excel](sql-database-connect-excel.md).
* Jeśli chcesz zacząć programować, wybierz język programowania w obszarze [Biblioteki połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md).
* Jeśli chcesz przenieść lokalne bazy danych programu SQL Server na platformę Azure, zobacz artykuł [Migrating a database to Azure SQL Database](sql-database-cloud-migrate.md) (Migracja bazy danych do usługi Azure SQL Database).
* Jeśli chcesz załadować dane z pliku CSV do nowej tabeli za pomocą narzędzia wiersza polecenia BCP, zobacz artykuł [Loading data into SQL Database from a CSV file using BCP](sql-database-load-from-csv-with-bcp.md) (Ładowanie danych do usługi SQL Database z pliku CSV za pomocą narzędzia BCP).
* Jeśli chcesz zacząć tworzyć tabele i inne obiekty, zobacz temat „Aby utworzyć tabelę” w artykule [Tworzenie tabeli](https://msdn.microsoft.com/library/ms365315.aspx).

## <a name="additional-resources"></a>Dodatkowe zasoby

- Aby zapoznać się z omówieniem technicznym, zobacz [Co to jest SQL Database?](sql-database-technical-overview.md)
- Aby uzyskać informacje o cenach, zobacz [Cennik usługi Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).




<!--HONumber=Feb17_HO3-->


