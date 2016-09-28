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
    ms.date="08/05/2016"
    ms.author="douglasl"/>


# Rozpoczynanie pracy przez uruchomienie kreatora włączania bazy danych Stretch

Aby skonfigurować bazę danych dla bazy danych Stretch, uruchom kreatora włączania bazy danych Stretch.  W tym temacie opisano informacje, które należy wprowadzić, oraz decyzje, które należy podjąć w kreatorze.

Aby dowiedzieć się więcej o bazie danych Stretch, zobacz artykuł [Baza danych Stretch](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Jeśli później wyłączysz usługę Stretch Database, pamiętaj, że jej wyłączenie dla tabeli lub bazy danych nie powoduje usunięcia obiektu zdalnego. Aby usunąć tabelę zdalną lub zdalną bazę danych, należy usunąć ją za pomocą portalu zarządzania Azure. Obiekty zdalne nadal generują koszty naliczane dla platformy Azure, dopóki nie zostaną usunięte ręcznie. 

## Uruchamianie Kreatora

1.  W programie SQL Server Management Studio w Eksploratorze obiektów wybierz bazę danych, dla której chcesz włączyć bazę danych Stretch.

2.  Kliknij prawym przyciskiem\- myszy i wybierz opcję **Tasks** (Zadania), następnie wybierz opcję **Stretch**, po czym kliknij **Enable** (Włącz), aby uruchomić kreatora.

## <a name="Intro"></a>Wprowadzenie
Zapoznaj się z celami kreatora i wymaganiami wstępnymi.

Poniżej przedstawiono ważne wymagania wstępne:

-   Należy być administratorem, aby zmienić ustawienia bazy danych.
-   Należy mieć subskrypcję platformy Microsoft Azure.
-   Program SQL Server musi mieć możliwość komunikowania się ze zdalnym serwerem Azure.

![Strona wprowadzenia kreatora bazy danych Stretch][StretchWizardImage1]

## <a name="Tables"></a>Wybór tabel
Wybierz tabele, które chcesz włączyć w bazie danych Stretch.

Tabele z dużą liczbą wierszy są wyświetlane u góry posortowanej listy. Zanim kreator wyświetli listę tabel, analizuje je pod kątem typów danych, które nie są obecnie obsługiwane przez usługę Stretch Database.

![Strona wybierania tabel kreatora bazy danych Stretch][StretchWizardImage2]

|Kolumna|Opis|
|----------|---------------|
|(bez tytułu)|Zaznacz pole wyboru w tej kolumnie, aby włączyć wybraną tabelę w bazie danych Stretch.|
|**Name (Nazwa)**|Określa nazwę kolumny w tabeli.|
|(bez tytułu)|Symbol w tej kolumnie może oznaczać ostrzeżenie, które nie\' uniemożliwia włączenia wybranej tabeli w usłudze Stretch. Może również oznaczać problem z blokowaniem, który nie pozwala włączyć wybranej tabeli w usłudze Stretch, \-co z kolei może być spowodowane użyciem w tabeli nieobsługiwanego typu danych. Umieść kursor na symbolu, aby wyświetlić więcej informacji w etykietce narzędzia. Aby uzyskać więcej informacji, zobacz temat [Limitations for Stretch Database](sql-server-stretch-database-limitations.md) (Ograniczenia dotyczące usługi Stretch Database).|
|**Stretched (Rozciągnięta)**|Wskazuje, czy tabela jest już włączona dla usługi Stretch.|
|**Migrate (Migracja)**|Możesz dokonać migracji całej tabeli (**Cała tabela**) albo określić funkcję filtru dla istniejącej kolumny w tabeli. Jeśli do wybrania wierszy do migracji chcesz użyć innej funkcji filtru, po zamknięciu kreatora uruchom instrukcję ALTER TABLE, aby określić funkcję filtru. Aby uzyskać więcej informacji na temat funkcji filtru, zobacz temat [Select rows to migrate by using a filter function](sql-server-stretch-database-predicate-function.md) (Korzystanie z funkcji filtru w celu wyboru wierszy do migrowania). Aby uzyskać więcej informacji dotyczących sposobu stosowania funkcji, zobacz artykuły [Enable Stretch Database for a table](sql-server-stretch-database-enable-table.md) (Włączanie usługi Stretch Database dla tabeli) lub [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Rows (Wiersze)**|Określa liczbę wierszy w tabeli.|
|**Size (Rozmiar) (KB)**|Określa rozmiar tabeli w KB.|

## <a name="Filter"></a>Opcjonalne określanie filtru wiersza

Jeśli chcesz podać predykat filtru w celu wybrania wierszy do migrowania, wykonaj następujące czynności na stronie **Select tables** (Wybieranie tabel).

1.  Na liście **Select the tables you want to stretch** (Wybierz tabele do rozciągania) kliknij pozycję **Entire Table** (Cała tabela) w wierszu odpowiadającym tabeli. Zostanie otwarte okno dialogowe **Select rows to stretch** (Wybieranie wierszy do rozciągnięcia).

    ![Definiowanie funkcji filtru][StretchWizardImage2a]

2.  W oknie dialogowym **Select rows to stretch** (Wybieranie wierszy do rozciągnięcia) wybierz opcję **Choose Rows** (Wybierz wiersze).

3.  W polu **Name** (Nazwa) podaj nazwę funkcji filtru.

4.  Dla klauzuli **Where** (Gdzie) wybierz z tabeli kolumnę, wybierz operator i określ wartość.

5. Kliknij przycisk **Check** (Sprawdź) w celu przetestowania funkcji. Jeśli funkcja zwraca wyniki z tabeli (czyli jeśli istnieją wiersze do migracji, które spełniają warunek), wynik testu to **Success** (Powodzenie).

    >   [AZURE.NOTE] Pole tekstowe zawierające zapytanie filtru jest tylko do odczytu. Nie można edytować zapytania w polu tekstowym.

6.  Kliknij przycisk Done (Gotowe), aby powrócić do strony **Select tables** (Wybieranie tabel).

Funkcja filtru jest tworzona w programie SQL Server tylko po zakończeniu pracy kreatora. Do tego czasu możesz wrócić do strony **Select tables** (Wybieranie tabel), aby zmienić funkcję filtru lub jej nazwę.

![Strona Select Tables (Wybieranie tabel) po zdefiniowaniu funkcji filtru][StretchWizardImage2b]

Jeśli chcesz użyć innej funkcji filtru w celu wybrania wierszy do migrowania, wykonaj jedną z następujących czynności.  

-   Zakończ działanie kreatora i uruchom instrukcję ALTER TABLE, aby włączyć usługę Stretch dla tabeli oraz określić funkcję filtru. Aby uzyskać więcej informacji, zobacz temat [Enable Stretch Database for a table](sql-server-stretch-database-enable-table.md) (Włączanie usługi Stretch dla tabeli).  

-   Uruchom instrukcję ALTER TABLE, aby określić funkcję filtru po zamknięciu kreatora. Wymagane czynności można znaleźć w części [Add a filter function after running the Wizard](sql-server-stretch-database-predicate-function.md#addafterwiz) (Dodawanie funkcji filtru po uruchomieniu kreatora).

## <a name="Configure"></a>Konfigurowanie wdrożenia usługi Azure

1.  Zaloguj się na platformie Microsoft Azure przy użyciu konta Microsoft.

    ![Logowanie na platformie Azure — kreator bazy danych Stretch][StretchWizardImage3]

2.  Wybierz istniejącą subskrypcję platformy Azure do użycia z usługą Stretch Database.

3.  Określ region platformy Azure.
    -   Jeśli tworzysz nowy serwer, zostanie on utworzony w tym regionie.  
    -   Jeśli masz istniejące serwery w wybranym regionie, kreator wyświetli je po wybraniu opcji **Existing server** (Istniejący serwer).

    Aby zminimalizować czas oczekiwania, wybierz region platformy Azure, w którym znajduje się serwer SQL Server. Aby uzyskać więcej informacji na temat regionów, zobacz temat [Regiony platformy Azure](https://azure.microsoft.com/regions/).

4.  Określ, czy chcesz użyć istniejącego serwera, czy też utworzyć nowy serwer Azure.

    Jeśli usługi Active Directory na serwerze SQL Server są sfederowane z usługą Azure Active Directory, można opcjonalnie użyć konta usługi federacyjnej dla serwera SQL Server do komunikowania się ze zdalnym serwerem Azure. Aby uzyskać więcej informacji o wymaganiach dotyczących tej opcji, zobacz temat [Opcje ALTER DATABASE SET (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Tworzenie nowego serwera**

        1.  Utwórz identyfikator logowania i hasło administratora serwera.

        2.  Opcjonalnie użyj konta usługi federacyjnej dla serwera SQL Server do komunikowania się ze zdalnym serwerem Azure.

        ![Tworzenie nowego serwera Azure — kreator bazy danych Stretch][StretchWizardImage4]

    -   **Istniejący serwer**

        1.  Wybierz istniejący serwer platformy Azure.

        2.  Wybierz metodę uwierzytelniania.

            -   W przypadku wybrania opcji **SQL Server Authentication** (Uwierzytelnianie programu SQL Server) utwórz nowy identyfikator logowania administratora i hasło.

            -   Wybierz opcję **Active Directory Integrated Authentication** (Zintegrowane uwierzytelnianie usługi Active Directory), aby używać konta usługi federacyjnej dla serwera SQL Server do komunikowania się ze zdalnym serwerem Azure. Jeśli wybrany serwer nie jest zintegrowany z usługą Azure Active Directory, ta opcja nie jest wyświetlana.

        ![Wybieranie istniejącego serwera Azure — kreator bazy danych Stretch][StretchWizardImage5]

## <a name="Credentials"></a>Bezpieczne poświadczenia
Musisz mieć klucz główny bazy danych, aby zabezpieczyć poświadczenia, których baza danych Stretch używa do nawiązywania połączenia ze zdalną bazą danych.  

Jeśli klucz główny bazy danych już istnieje, wprowadź dla niego hasło.  

![Strona bezpiecznych poświadczeń kreatora bazy danych Stretch][StretchWizardImage6b]

Jeśli baza danych nie ma istniejącego klucza głównego, wpisz silne hasło, aby utworzyć klucz główny bazy danych.  

![Strona bezpiecznych poświadczeń kreatora bazy danych Stretch][StretchWizardImage6]

Aby uzyskać więcej informacji na temat klucza głównego bazy danych, zobacz artykuły [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) i [Tworzenie klucza głównego bazy danych](https://msdn.microsoft.com/library/aa337551.aspx). Aby uzyskać więcej informacji o poświadczeniach utworzonych przez kreatora, zobacz artykuł [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Wybieranie adresu IP
Użyj zakresu adresów IP podsieci (zalecane) lub publicznego adresu IP serwera SQL Server, aby utworzyć regułę zapory platformy Azure umożliwiającą serwerowi SQL Server komunikowanie się ze zdalnym serwerem Azure.

Adres IP lub adresy podane na tej stronie instruują serwer platformy Azure, aby zezwalał ma przekazywanie przez zaporę platformy Azure przychodzących danych, zapytań i operacji zarządzania inicjowanych przez serwer SQL Server. Kreator nie zmienia żadnych ustawień zapory na serwerze SQL Server.

![Strona wybierania adresu IP kreatora bazy danych Stretch][StretchWizardImage7]

## <a name="Summary"></a>Podsumowanie
Przejrzyj wprowadzone wartości i opcje wybrane w kreatorze oraz szacowane koszty na platformie Azure. Następnie wybierz opcję **Finish** (Zakończ), aby włączyć bazę danych Stretch.

![Strona podsumowania kreatora bazy danych Stretch][StretchWizardImage8]

## <a name="Results"></a>Wyniki
Przejrzyj wyniki.

Aby monitorować stan migracji danych, zobacz temat [Monitor and troubleshoot data migration (Stretch Database)](sql-server-stretch-database-monitor.md) (Monitorowanie migracji danych i rozwiązywanie problemów — Stretch Database).

![Strona wyników kreatora usługi Stretch Database][StretchWizardImage9]

## <a name="KnownIssues"></a>Rozwiązywanie problemów z kreatorem
**Działanie kreatora bazy danych Stretch zakończyło się niepowodzeniem.**
Jeśli baza danych Stretch nie została włączona na poziomie serwera, a kreator został uruchomiony bez uprawnień administratora systemu do jej włączenia, działanie kreatora zakończy się niepowodzeniem. Poproś administratora systemu o włączenie bazy danych Stretch w lokalnym wystąpieniu serwera, a następnie ponownie uruchom kreatora. Aby uzyskać więcej informacji, zobacz artykuł [Wymagania wstępne: uprawnienia do włączania bazy danych Stretch na serwerze](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## Następne kroki
Włącz dodatkowe tabele dla bazy danych Stretch. Monitoruj migrację danych i zarządzaj \-włączonymi bazami danych Stretch i tabelami.

-   Temat [Włączanie bazy danych Stretch dla tabeli](sql-server-stretch-database-enable-table.md) zawiera informacje na temat włączania dodatkowych tabel.

-   Temat [Monitor and troubleshoot data migration](sql-server-stretch-database-monitor.md) (Monitorowanie migracji danych i rozwiązywanie problemów) zawiera informacje na temat stanu migracji danych.

-   [Wstrzymywanie i wznawianie bazy danych Stretch](sql-server-stretch-database-pause.md)

-   [Zarządzanie bazą danych Stretch i rozwiązywanie problemów](sql-server-stretch-database-manage.md)

-   [Tworzenie kopii zapasowych baz danych z obsługą usługi Stretch](sql-server-stretch-database-backup.md)

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
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png



<!--HONumber=Sep16_HO3-->


