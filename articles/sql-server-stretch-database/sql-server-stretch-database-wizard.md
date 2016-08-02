<properties
    pageTitle="Rozpoczynanie pracy przez uruchomienie kreatora włączania bazy danych Stretch | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować bazę danych dla bazy danych Stretch przy użyciu kreatora włączania bazy danych Stretch."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager=""
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/17/2016"
    ms.author="douglasl"/>

# Rozpoczynanie pracy przez uruchomienie kreatora włączania bazy danych Stretch

Aby skonfigurować bazę danych dla bazy danych Stretch, uruchom kreatora włączania bazy danych Stretch.  W tym temacie opisano informacje, które należy wprowadzić, oraz decyzje, które należy podjąć w kreatorze.

Aby dowiedzieć się więcej o bazie danych Stretch, zobacz artykuł [Baza danych Stretch](sql-server-stretch-database-overview.md).

## Uruchamianie Kreatora

1.  W programie SQL Server Management Studio w Eksploratorze obiektów wybierz bazę danych, dla której chcesz włączyć bazę danych Stretch.

2.  Kliknij prawym przyciskiem myszy i wybierz opcję **Tasks** (Zadania), następnie wybierz opcję **Stretch**, po czym kliknij **Enable** (Włącz), aby uruchomić kreatora.

## <a name="Intro"></a>Wprowadzenie
Zapoznaj się z celami kreatora i wymaganiami wstępnymi.

![Strona wprowadzenia kreatora bazy danych Stretch][StretchWizardImage1]

## <a name="Tables"></a>Wybór tabel
Wybierz tabele, które chcesz włączyć w bazie danych Stretch.

![Strona wybierania tabel kreatora bazy danych Stretch][StretchWizardImage2]

|Kolumna|Opis|
|----------|---------------|
|(bez tytułu)|Zaznacz pole wyboru w tej kolumnie, aby włączyć wybraną tabelę w bazie danych Stretch.|
|**Name (Nazwa)**|Określa nazwę kolumny w tabeli.|
|(bez tytułu)|Symbol w tej kolumnie zwykle oznacza, że wybranej tabeli nie można włączyć w bazie danych Stretch z powodu problemu z blokowaniem. Może to być spowodowane użyciem w tabeli nieobsługiwanego typu danych. Umieść kursor na symbolu, aby wyświetlić więcej informacji w etykietce narzędzia. Aby uzyskać więcej informacji, zobacz temat [Ograniczenia obszaru powierzchni i problemy z blokowaniem w bazie danych Stretch](sql-server-stretch-database-limitations.md).|
|**Stretched (Rozciągnięta)**|Wskazuje, że tabela jest już włączona.|
|**Migrate (Migracja)**|Możesz dokonać migracji całej tabeli (**Entire Table**) albo określić w kreatorze predykat filtru oparty na dacie. Jeśli do wybrania wierszy do migracji chcesz użyć innego predykatu filtru, po zamknięciu kreatora uruchom instrukcję ALTER TABLE, aby określić predykat filtru. Aby uzyskać więcej informacji na temat predykatu filtru, zobacz temat [Korzystanie z predykatu filtru w celu wyboru wierszy do migrowania (baza danych Stretch)](sql-server-stretch-database-predicate-function.md). Aby uzyskać więcej informacji dotyczących sposobu stosowania predykatu, zobacz artykuły [Włączanie bazy danych Stretch dla tabeli](sql-server-stretch-database-enable-table.md) lub [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Rows (Wiersze)**|Określa liczbę wierszy w tabeli.|
|**Size (Rozmiar) (KB)**|Określa rozmiar tabeli w KB.|

## <a name="Filter"></a>Opcjonalne określanie predykatu filtru opartego na dacie

Jeśli chcesz podać predykat filtru oparty na dacie w celu wybrania wierszy do migrowania, wykonaj następujące czynności na stronie **Select tables** (Wybieranie tabel).

1.  Na liście **Select the tables you want to stretch** (Wybierz tabele do rozciągania) kliknij pozycję **Entire Table** (Cała tabela) w wierszu odpowiadającym tabeli. Zostanie otwarte okno dialogowe **Select rows to stretch** (Wybieranie wierszy do rozciągnięcia).

    ![Definiowanie predykatu filtru opartego o datę][StretchWizardImage2a]

2.  W oknie dialogowym **Select rows to stretch** (Wybieranie wierszy do rozciągnięcia) wybierz opcję **Choose Rows** (Wybierz wiersze).

3.  W polu **Name** (Nazwa) podaj nazwę predykatu filtru.

4.  Dla klauzuli **Where** (Gdzie) wybierz z tabeli kolumnę daty, wybierz operator i określ wartość daty.

5. Kliknij przycisk **Check** (Sprawdź) w celu przetestowania predykatu. Jeśli predykat zwraca wyniki z tabeli (czyli jeśli istnieją wiersze do migracji, które spełniają warunek), wynik testu to **Success** (Powodzenie).

6.  Kliknij przycisk Done (Gotowe), aby powrócić do strony **Select tables** (Wybieranie tabel).

    ![Wybieranie strony Tables (Tabele) po zdefiniowaniu predykatu filtru][StretchWizardImage2b]

## <a name="Configure"></a>Konfigurowanie wdrożenia usługi Azure

1.  Zaloguj się na platformie Microsoft Azure przy użyciu konta Microsoft.

    ![Logowanie na platformie Azure — kreator bazy danych Stretch][StretchWizardImage3]

2.  Wybierz subskrypcję platformy Azure na potrzeby bazy danych Stretch.

3.  Określ region platformy Azure. Jeśli tworzysz nowy serwer, zostanie on utworzony w tym regionie.

    Aby zminimalizować czas oczekiwania, wybierz region platformy Azure, w którym znajduje się serwer SQL Server. Aby uzyskać więcej informacji na temat regionów, zobacz temat [Regiony platformy Azure](https://azure.microsoft.com/regions/).

4.  Określ, czy chcesz użyć istniejącego serwera, czy też utworzyć nowy serwer Azure.

    Jeśli usługi Active Directory na serwerze SQL Server są sfederowane z usługą Azure Active Directory, można opcjonalnie użyć konta usługi federacyjnej dla serwera SQL Server do komunikowania się ze zdalnym serwerem Azure. Aby uzyskać więcej informacji o wymaganiach dotyczących tej opcji, zobacz temat [Opcje ALTER DATABASE SET (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Tworzenie nowego serwera**

        1.  Utwórz identyfikator logowania i hasło administratora serwera.

        2.  Opcjonalnie użyj konta usługi federacyjnej dla serwera SQL Server do komunikowania się ze zdalnym serwerem Azure.

        ![Tworzenie nowego serwera Azure — kreator bazy danych Stretch][StretchWizardImage4]

    -   **Istniejący serwer**

        1.  Wybierz lub wprowadź nazwę istniejącego serwera platformy Azure.

        2.  Wybierz metodę uwierzytelniania.

            -   W przypadku wybrania opcji **SQL Server Authentication** (Uwierzytelnianie programu SQL Server) utwórz nowy identyfikator logowania i hasło.

            -   Wybierz opcję **Active Directory Integrated Authentication** (Zintegrowane uwierzytelnianie usługi Active Directory), aby używać konta usługi federacyjnej dla serwera SQL Server do komunikowania się ze zdalnym serwerem Azure.

        ![Wybieranie istniejącego serwera Azure — kreator bazy danych Stretch][StretchWizardImage5]

## <a name="Credentials"></a>Bezpieczne poświadczenia
Wpisz silne hasło, aby utworzyć klucz główny bazy danych, lub jeśli klucz główny bazy danych już istnieje, wprowadź hasło klucza.

Musisz mieć klucz główny bazy danych, aby zabezpieczyć poświadczenia, których baza danych Stretch używa do nawiązywania połączenia ze zdalną bazą danych.

![Strona bezpiecznych poświadczeń kreatora bazy danych Stretch][StretchWizardImage6]

Aby uzyskać więcej informacji na temat klucza głównego bazy danych, zobacz artykuły [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) i [Tworzenie klucza głównego bazy danych](https://msdn.microsoft.com/library/aa337551.aspx). Aby uzyskać więcej informacji o poświadczeniach utworzonych przez kreatora, zobacz artykuł [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Wybieranie adresu IP
Użyj publicznego adresu IP serwera SQL Server, lub wprowadź zakres adresów IP, aby utworzyć regułę zapory platformy Azure umożliwiającą serwerowi SQL Server komunikowanie się ze zdalnym serwerem Azure.

Adres IP lub adresy podane na tej stronie instruują serwer platformy Azure, aby zezwalał ma przekazywanie przez zaporę platformy Azure przychodzących danych, zapytań i operacji zarządzania inicjowanych przez serwer SQL Server. Kreator nie zmienia żadnych ustawień zapory na serwerze SQL Server.

![Strona wybierania adresu IP kreatora bazy danych Stretch][StretchWizardImage7]

## <a name="Summary"></a>Podsumowanie
Przejrzyj wybrane w kreatorze opcje i wprowadzone wartości. Następnie wybierz opcję **Finish** (Zakończ), aby włączyć bazę danych Stretch.

![Strona podsumowania kreatora bazy danych Stretch][StretchWizardImage8]

## <a name="Results"></a>Wyniki
Przejrzyj wyniki.

Opcjonalnie zaznacz pole wyboru **Monitor** (Monitorowanie), aby uruchomić monitor stanu migracji danych w monitorze bazy danych Stretch. Aby uzyskać więcej informacji, zobacz artykuł [Monitorowanie migracji danych i rozwiązywanie problemów (baza danych Stretch)](sql-server-stretch-database-monitor.md).

## <a name="KnownIssues"></a>Rozwiązywanie problemów z kreatorem
**Działanie kreatora bazy danych Stretch zakończyło się niepowodzeniem.**
Jeśli baza danych Stretch nie została włączona na poziomie serwera, a kreator został uruchomiony bez uprawnień administratora systemu do jej włączenia, działanie kreatora zakończy się niepowodzeniem. Poproś administratora systemu o włączenie bazy danych Stretch w lokalnym wystąpieniu serwera, a następnie ponownie uruchom kreatora. Aby uzyskać więcej informacji, zobacz artykuł [Wymagania wstępne: uprawnienia do włączania bazy danych Stretch na serwerze](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## Następne kroki
Włącz dodatkowe tabele dla bazy danych Stretch. Monitoruj migrację danych i zarządzaj włączonymi bazami danych Stretch i tabelami.

-   Temat [Włączanie bazy danych Stretch dla tabeli](sql-server-stretch-database-enable-table.md) zawiera informacje na temat włączania dodatkowych tabel.

-   Temat [Monitorowanie bazy danych Stretch](sql-server-stretch-database-monitor.md) zawiera informacje na temat sprawdzania stanu migracji danych.

-   [Wstrzymywanie i wznawianie bazy danych Stretch](sql-server-stretch-database-pause.md)

-   [Zarządzanie bazą danych Stretch i rozwiązywanie problemów](sql-server-stretch-database-manage.md)

-   [Tworzenie kopii zapasowych i przywracanie baz danych Stretch](sql-server-stretch-database-backup.md)

## Zobacz też

[Włączanie bazy danych Stretch dla bazy danych](sql-server-stretch-database-enable-database.md)

[Włączanie bazy danych Stretch dla tabeli](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png



<!--HONumber=Jun16_HO2-->


