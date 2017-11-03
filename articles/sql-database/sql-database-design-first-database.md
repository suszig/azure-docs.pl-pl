---
title: "Projektowanie pierwszą bazę danych Azure SQL | Dokumentacja firmy Microsoft"
description: "Dowiedz się zaprojektować pierwszą bazę danych Azure SQL w portalu Azure i SQL Server Management Studio."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop databases
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 08/25/2017
ms.author: carlrab
ms.openlocfilehash: e4848eb366faea134a484c8a494fed6a83203116
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="design-your-first-azure-sql-database"></a>Projektowanie pierwszą bazę danych Azure SQL

Baza danych SQL Azure to relacyjnej bazy danych — jako a usługa (DBaaS) w Microsoft Cloud ("Azure"). W tym samouczku, możesz dowiedzieć się, jak korzystać z portalu Azure i [programu SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) do: 

> [!div class="checklist"]
> * Utwórz bazę danych w portalu Azure
> * Reguły zapory poziomu serwera w portalu Azure
> * Połączenie z bazą danych z narzędzia SSMS
> * Tworzenie tabel z SSMS
> * Danych ładowania zbiorczego, za pomocą narzędzia BCP
> * Zapytanie danych z narzędzia SSMS
> * Przywróć bazę danych do poprzedniej [punktu w czasie przywracania](sql-database-recovery-using-backups.md#point-in-time-restore) w portalu Azure

Jeśli nie masz subskrypcji platformy Azure, [utworzyć bezpłatne konto](https://azure.microsoft.com/free/) przed rozpoczęciem.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka, upewnij się, że jest zainstalowana:
- Najnowsza wersja [programu SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS).
- Najnowsza wersja [BCP i SQLCMD](https://www.microsoft.com/download/details.aspx?id=36433).

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-blank-sql-database"></a>Utwórz pustą bazę danych SQL

Baza danych Azure SQL jest tworzona ze zdefiniowanym zestawem [zasobów obliczeniowych i przechowywania](sql-database-service-tiers.md). Baza danych jest tworzona w [grupie zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) oraz na [serwerze logicznym bazy danych Azure SQL Database](sql-database-features.md). 

Wykonaj następujące kroki, aby utworzyć pustą bazę danych SQL. 

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Nowy** w obszarze **SQL Database** wybierz pozycję **Utwórz**.

   ![Tworzenie bazy danych puste](./media/sql-database-design-first-database/create-empty-database.png)

3. Wypełnij formularz Baza danych SQL w sposób pokazany na wcześniejszej ilustracji, używając następujących informacji:   

   | Ustawienie       | Sugerowana wartość | Opis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nazwa bazy danych** | mySampleDatabase | Prawidłowe nazwy baz danych opisano w artykule [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identyfikatory baz danych). | 
   | **Subskrypcja** | Twoja subskrypcja  | Aby uzyskać szczegółowe informacje o subskrypcjach, zobacz [Subskrypcje](https://account.windowsazure.com/Subscriptions). |
   | **Grupa zasobów** | myResourceGroup | Prawidłowe nazwy grup zasobów opisano w artykule [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Reguły i ograniczenia nazewnictwa). |
   | **Wybierz źródło** | Pusta baza danych | Określa, czy można utworzyć pustej bazy danych. |

4. Kliknij pozycję **Serwer**, aby utworzyć i skonfigurować nowy serwer dla nowej bazy danych. Wypełnianie **nowy formularz serwera** z następującymi informacjami: 

   | Ustawienie       | Sugerowana wartość | Opis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nazwa serwera** | Dowolna nazwa unikatowa w skali globalnej | Prawidłowe nazwy serwera opisano w artykule [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Reguły i ograniczenia nazewnictwa). | 
   | **Identyfikator logowania administratora serwera** | Dowolna prawidłowa nazwa | Prawidłowe nazwy identyfikatorów logowania opisano w artykule [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identyfikatory baz danych).|
   | **Hasło** | Dowolne prawidłowe hasło | Hasło musi mieć co najmniej ośmiu znaków i musi zawierać znaki z trzech z następujących kategorii: wielkich liter, małych liter, cyfr i znaków innych niż alfanumeryczne. |
   | **Lokalizacja** | Dowolna prawidłowa lokalizacja | Aby uzyskać informacje na temat regionów, zobacz temat [Regiony systemu Azure](https://azure.microsoft.com/regions/). |

   ![tworzenie serwera bazy danych](./media/sql-database-design-first-database/create-database-server.png)

5. Kliknij pozycję **Wybierz**.

6. Kliknij pozycję **Warstwa cenowa**, aby określić warstwę usługi, liczbę jednostek DTU i ilość miejsca do magazynowania. Zapoznaj się z opcjami liczby jednostek Dtu i Magazyn, które są dostępne dla każdej warstwy usług. 

7. W tym samouczku, wybierz **standardowe** warstwę usługi, a następnie wybierz za pomocą suwaka **100 Dtu (S3)** i **400** GB miejsca do magazynowania.

   ![tworzenie bazy danych s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. Zaakceptuj warunki wersji zapoznawczej, aby użyć opcji **dodatkowego magazynu**. 

   > [!IMPORTANT]
   > \* Magazyn o rozmiarze większym niż ilość miejsca do magazynowania są dostępne w wersji zapoznawczej dodatkowych kosztów za dodatkową opłatą. Szczegóły można znaleźć w [cenniku usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* W warstwie Premium magazyn o rozmiarze ponad 1 TB jest obecnie dostępny w następujących regionach: Wschodnie stany USA 2, Zachodnie stany USA, Administracja USA — Wirginia, Europa Zachodnia, Niemcy Środkowe, Azja Południowo-Wschodnia, Japonia Wschodnia, Australia Wschodnia, Kanada Środkowa i Kanada Wschodnia. Więcej informacji można znaleźć na stronie [bieżących ograniczeń poziomów P11–P15](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Po wybraniu warstwy serwera, liczby jednostek DTU i ilości miejsca do magazynowania kliknij przycisk **Zastosuj**.  

10. Wybierz **sortowania** dla pustą bazę danych (w tym samouczku, użyj wartości domyślnej). Aby uzyskać więcej informacji na temat sortowań zobacz [sortowania](https://docs.microsoft.com/sql/t-sql/statements/collations)

11. Teraz po uzupełnieniu formularza SQL Database kliknij przycisk **Utwórz**, aby aprowizować bazę danych. Aprowizacja zajmuje kilka minut. 

12. Na pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.
    
     ![powiadomienie](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Tworzenie reguły zapory na poziomie serwera

Usługa SQL Database tworzy zaporę na poziomie serwera, która uniemożliwia zewnętrznym aplikacjom i narzędziom łączenie się z serwerem i wszelkimi bazami danych na tym serwerze, chyba że zostanie utworzona reguła zapory otwierająca zaporę dla konkretnych adresów IP. Wykonaj następujące kroki, aby utworzyć [regułę zapory na poziomie serwera usługi SQL Database](sql-database-firewall-configure.md) dla podanego adresu IP klienta i włączyć zewnętrzną łączność przez zaporę usługi SQL Database wyłącznie dla konkretnego adresu IP. 

> [!NOTE]
> Usługa SQL Database nawiązuje komunikację na porcie 1433. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 1433 może być zablokowany przez firmową zaporę. Jeśli nastąpi taka sytuacja, nie będzie można nawiązać połączenia z serwerem usługi Azure SQL Database, chyba że dział IT otworzy port 1433.
>

1. Po ukończeniu wdrażania kliknij pozycję **Bazy danych SQL** w menu po lewej stronie i kliknij bazę danych **mySampleDatabase** na stronie **Bazy danych SQL**. Zostanie otwarta strona przeglądu bazy danych zawierająca w pełni kwalifikowaną nazwę serwera (na przykład **mynewserver-20170824.database.windows.net**) i opcje dalszej konfiguracji. 

2. Skopiuj tę w pełni kwalifikowaną nazwę serwera w celu nawiązania połączenia z serwerem i jego bazami danych w kolejnych przewodnikach Szybki start. 

   ![nazwa serwera](./media/sql-database-get-started-portal/server-name.png) 

3. Kliknij przycisk **ustawić Zapora serwera** na pasku narzędzi. Zostanie otwarta strona **Ustawienia zapory** dla serwera SQL Database. 

   ![reguła zapory serwera](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Kliknij pozycję **Dodaj adres IP klienta** na pasku narzędzi, aby dodać bieżący adres IP do nowej reguły zapory. Reguła zapory może otworzyć port 1433 dla pojedynczego adresu IP lub zakresu adresów IP.

5. Kliknij pozycję **Zapisz**. Dla bieżącego adresu IP zostanie utworzona reguła zapory na poziomie serwera otwierająca port 1433 na serwerze logicznym.

6. Kliknij przycisk **OK**, a następnie zamknij stronę **Ustawienia zapory**.

Teraz można połączyć się z serwerem usługi SQL Database i jego bazami danych przy użyciu programu SQL Server Management Studio lub innego wybranego narzędzia z tego adresu IP przy użyciu poprzednio utworzonego konta administratora serwera.

> [!IMPORTANT]
> Domyślnie dostęp za pośrednictwem zapory usługi SQL Database jest włączony dla wszystkich usług platformy Azure. Kliknij przycisk **WYŁ.** na tej stronie, aby wyłączyć tę opcję dla wszystkich usług platformy Azure.

## <a name="sql-server-connection-information"></a>Informacje o połączeniu z serwerem SQL

Pobierz w pełni kwalifikowaną nazwę serwera dla serwera Azure SQL Database w witrynie Azure Portal. W pełni kwalifikowanej nazwy serwera używa się do nawiązywania połączenia z serwerem przy użyciu programu SQL Server Management Studio.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. W okienku **Essentials** na stronie bazy danych w witrynie Azure Portal zlokalizuj i skopiuj **nazwę serwera**.

   ![informacje o połączeniu](./media/sql-database-get-started-portal/server-name.png)

## <a name="connect-to-the-database-with-ssms"></a>Połączenie z bazą danych z narzędzia SSMS

Użyj [programu SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) nawiązać połączenia z serwerem bazy danych SQL Azure.

1. Otwórz program SQL Server Management Studio.

2. W oknie dialogowym **Połącz z serwerem** wprowadź następujące informacje:

   | Ustawienie       | Sugerowana wartość | Opis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Typ serwera | Aparat bazy danych | Ta wartość jest wymagana |
   | Nazwa serwera | W pełni kwalifikowana nazwa serwera | Nazwa powinna być podobny do następującego: **mynewserver20170824.database.windows.net**. |
   | Authentication | Uwierzytelnianie programu SQL Server | Uwierzytelnianie SQL to jedyny typ uwierzytelniania skonfigurowany w tym samouczku. |
   | Login | Konto administratora serwera | To konto określono podczas tworzenia serwera. |
   | Hasło | Hasło konta administratora serwera | To hasło określono podczas tworzenia serwera. |

   ![łączenie z serwerem](./media/sql-database-connect-query-ssms/connect.png)

3. Kliknij przycisk **Opcje** w oknie dialogowym **Połącz z serwerem**. W sekcji **Nawiązywanie połączenia z bazą danych** wprowadź ciąg **mySampleDatabase**, aby nawiązać połączenie z tą bazą danych.

   ![nawiązywanie połączenia z bazą danych na serwerze](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Kliknij przycisk **Połącz**. W programie SSMS zostanie otwarte okno Eksplorator obiektów. 

5. W Eksploratorze obiektów rozwiń pozycję **Bazy danych**, a następnie rozwiń pozycję **mySampleDatabase**, aby wyświetlić obiekty w przykładowej bazie danych.

   ![Obiekty bazy danych](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="create-tables-in-the-database"></a>Tworzenie tabel w bazie danych 

Tworzenie schematu bazy danych z czterech tabel, które system zarządzania uczniów uczelni przy użyciu modelu [języka Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference):

- Osoby
- Plan
- Dla użytkowników domowych
- System zarządzania uczniów uczelni modelu karty kredytowej

Na poniższym diagramie przedstawiono, jak te tabele są ze sobą powiązane. Niektóre z tych tabel odwoływać się do kolumn w innych tabelach. Na przykład odwołuje się do tabeli uczniowie **PersonId** kolumny **osoby** tabeli. Badanie na diagramie, aby zrozumieć, jak tabele w tym samouczku są ze sobą powiązane. Aby omówiono sposób tworzenia tabel skuteczne bazy danych, zobacz [tworzenia tabel bazy danych skuteczne](https://msdn.microsoft.com/library/cc505842.aspx). Aby uzyskać informacje dotyczące wybierania typów danych, zobacz [typy danych](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> Można również użyć [projektanta tabel w programie SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) tworzenie i projektowanie tabelach. 

![Relacje między tabelami](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **mySampleDatabase** i kliknij opcję **Nowe zapytanie**. Zostanie otwarte puste okno zapytania, które jest połączone z Twoją bazą danych.

2. W oknie zapytania wykonaj następujące zapytanie, aby utworzyć cztery tabele w bazie danych: 

   ```sql 
   -- Create Person table

   CREATE TABLE Person
   (
   PersonId   INT IDENTITY PRIMARY KEY,
   FirstName   NVARCHAR(128) NOT NULL,
   MiddelInitial NVARCHAR(10),
   LastName   NVARCHAR(128) NOT NULL,
   DateOfBirth   DATE NOT NULL
   )
   
   -- Create Student table
 
   CREATE TABLE Student
   (
   StudentId INT IDENTITY PRIMARY KEY,
   PersonId  INT REFERENCES Person (PersonId),
   Email   NVARCHAR(256)
   )
   
   -- Create Course table
 
   CREATE TABLE Course
   (
   CourseId  INT IDENTITY PRIMARY KEY,
   Name   NVARCHAR(50) NOT NULL,
   Teacher   NVARCHAR(256) NOT NULL
   ) 

   -- Create Credit table
 
   CREATE TABLE Credit
   (
   StudentId   INT REFERENCES Student (StudentId),
   CourseId   INT REFERENCES Course (CourseId),
   Grade   DECIMAL(5,2) CHECK (Grade <= 100.00),
   Attempt   TINYINT,
   CONSTRAINT  [UQ_studentgrades] UNIQUE CLUSTERED
   (
   StudentId, CourseId, Grade, Attempt
   )
   )
   ```

   ![Tworzenie tabel](./media/sql-database-design-first-database/create-tables.png)

3. Rozwiń węzeł "tabele" w Eksploratorze programu SQL Server Management Studio obiektu, aby zobaczyć tabele, który został utworzony.

   ![tabele utworzone w programie ssms](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Ładowanie danych do tabel

1. Utwórz folder o nazwie **SampleTableData** w folderze pobrane do przechowywania przykładowych danych bazy danych. 

2. Kliknij prawym przyciskiem myszy poniższe łącza, a następnie zapisać je w **SampleTableData** folderu. 

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Otwórz okno wiersza polecenia i przejdź do folderu SampleTableData.

4. Wykonaj następujące polecenia, aby wstawić przykładowych danych do tabel, zastępując wartości **ServerName**, **DatabaseName**, **UserName**, i  **Hasło** wartościami dla danego środowiska.
  
   ```bcp
   bcp Course in SampleCourseData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

Teraz załadowano przykładowych danych do tabel, który został utworzony wcześniej.

## <a name="query-data"></a>Zapytania o dane

Wykonaj następujące kwerendy, aby pobrać informacje z tabel bazy danych. Zobacz [Pisanie zapytań SQL](https://technet.microsoft.com/library/bb264565.aspx) Aby dowiedzieć się więcej na temat Pisanie zapytań SQL. Pierwsza kwerenda łączy wszystkie cztery tabele, aby znaleźć wszystkie studentów nauczanych przez "Dominick Pope", którzy mają wyższe niż 75% stopnia w swojej klasie. Dołącza wszystkie cztery tabele drugiego zapytania i klient znajdzie wszystkich kursów, w których kiedykolwiek zarejestrował "Noe Coleman".

1. W oknie zapytania SQL Server Management Studio wykonaj następujące zapytanie:

   ```sql 
   -- Find the students taught by Dominick Pope who have a grade higher than 75%

   SELECT  person.FirstName,
   person.LastName,
   course.Name,
   credit.Grade
   FROM  Person AS person
   INNER JOIN Student AS student ON person.PersonId = student.PersonId
   INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
   INNER JOIN Course AS course ON credit.CourseId = course.courseId
   WHERE course.Teacher = 'Dominick Pope' 
   AND Grade > 75
   ```

2. W oknie zapytania SQL Server Management Studio wykonaj następujące zapytanie:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled

   SELECT  course.Name,
   course.Teacher,
   credit.Grade
   FROM  Course AS course
   INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
   INNER JOIN Student AS student ON student.StudentId = credit.StudentId
   INNER JOIN Person AS person ON person.PersonId = student.PersonId
   WHERE person.FirstName = 'Noe'
   AND person.LastName = 'Coleman'
   ```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Przywracanie bazy danych do określonego punktu w czasie

Załóżmy, że przypadkowo usunięto tabelę. Jest to coś, co nie można łatwo odzyskać z. Baza danych SQL Azure umożliwia wróć do dowolnego punktu w czasie w górę ostatnich 35 dni i przywrócić ten punkt w czasie do nowej bazy danych. Można odzyskać danych usuniętych tej bazy danych. Poniższe kroki przywrócenie przykładowej bazy danych do punktu przed dodaniem tabel.

1. Na stronie Baza danych SQL dla bazy danych, kliknij przycisk **przywrócić** na pasku narzędzi. **Przywrócić** zostanie otwarta strona.

   ![Przywracanie](./media/sql-database-design-first-database/restore.png)

2. Wypełnianie **przywrócić** formularza z informacjami wymaganymi:
    * Nazwa bazy danych: podaj nazwę bazy danych 
    * W momencie: Wybierz **w momencie** kartę w formularzu przywracania 
    * Punkt przywracania: Wybierz godzinę, która występuje przed zmianą bazy danych
    * Serwer docelowy: nie można zmienić tę wartość podczas przywracania bazy danych 
    * Elastyczna pula baz danych: Wybierz **None**  
    * Warstwa cenowa: Wybierz **20 jednostek Dtu** i **40 GB** magazynu.

   ![punkt przywracania](./media/sql-database-design-first-database/restore-point.png)

3. Kliknij przycisk **OK** Aby przywrócić bazę danych do [Przywracanie do punktu w czasie](sql-database-recovery-using-backups.md#point-in-time-restore) przed dodaniem tabel. Przywracanie bazy danych do innego punktu w czasie tworzy duplikat bazy danych w tym samym serwerze co oryginalnej bazy danych, począwszy od punktu w czasie, możesz określić, tak długo, jak jest w okresie przechowywania dla Twojego [warstwy usług](sql-database-service-tiers.md).

## <a name="next-steps"></a>Następne kroki 
W tym samouczku przedstawiono bazy danych podstawowych zadań takich jak utworzyć bazę danych i tabele, załadować zapytania na danych i Przywróć bazę danych z wcześniejszego punktu w czasie. W tym samouczku omówiono:
> [!div class="checklist"]
> * Tworzenie bazy danych
> * Konfigurowanie reguł zapory
> * Połączenie z bazą danych z [programu SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS)
> * Tworzenie tabel
> * Danych ładowania zbiorczego
> * Dane zapytania
> * Przywróć bazę danych z wcześniejszego punktu w czasie przy użyciu bazy danych SQL [punktu w czasie przywracania](sql-database-recovery-using-backups.md#point-in-time-restore) możliwości

Przejdź do następnego samouczek, aby dowiedzieć się więcej o projektowaniu bazy danych przy użyciu programu Visual Studio i C#.

> [!div class="nextstepaction"]
>[Projekt bazy danych Azure SQL i Połącz z C# i ADO.NET](sql-database-design-first-database-csharp.md)
