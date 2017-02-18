---
title: "Azure SQL Data Warehouse — samouczek z wprowadzeniem | Microsoft Docs"
description: "W tym samouczku pokazano, jak aprowizować usługę Azure SQL Data Warehouse i ładować do niej dane. Przedstawiono w nim też podstawowe informacje dotyczące skalowania, wstrzymywania i dostosowywania."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/26/2017
ms.author: elbutter;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2c88c1abd2af7a1ca041cd5003fd1f848e1b311c
ms.openlocfilehash: 12f72e76ee991dfb701637847f2e406cd0f8c449


---
# <a name="get-started-with-sql-data-warehouse"></a>Rozpoczynanie pracy z usługą SQL Data Warehouse

W tym samouczku pokazano, jak aprowizować usługę Azure SQL Data Warehouse i ładować do niej dane. Przedstawiono w nim też podstawowe informacje dotyczące skalowania, wstrzymywania i dostosowywania. Ten samouczek przygotuje Cię do eksplorowania magazynu danych i wykonywania w nim zapytań.

**Szacowany czas trwania:** jest to kompleksowy samouczek z przykładowym kodem, który trwa około 30 minut, gdy spełniono wymagania wstępne. 

## <a name="prerequisites"></a>Wymagania wstępne

W samouczku przyjęto założenie, że znasz podstawowe pojęcia związane z usługą SQL Data Warehouse. Wprowadzenie można znaleźć na stronie [Co to jest usługa SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md) 

### <a name="sign-up-for-microsoft-azure"></a>Utwórz konto na platformie Microsoft Azure
Jeśli nie masz jeszcze konta platformy Microsoft Azure, musisz je utworzyć, aby móc używać tej usługi. Jeśli masz już konto platformy Azure, pomiń ten krok. 

1. Przejdź do strony konta [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)
2. Utwórz bezpłatne konto platformy Azure lub zakup konto.
3. Postępuj zgodnie z instrukcjami

### <a name="install-appropriate-sql-client-drivers-and-tools"></a>Instalowanie odpowiednich sterowników i narzędzi klienta SQL

Większość narzędzi klienta SQL może nawiązywać połączenie z usługą SQL Data Warehouse przy użyciu sterownika JDBC, ODBC lub ADO.NET. Z powodu dużej liczby funkcji w języku T-SQL obsługiwanych przez usługę SQL Data Warehouse niektóre aplikacje klienckie nie są w pełni zgodne z usługą SQL Data Warehouse.

Jeśli używasz systemu operacyjnego Windows, zalecamy użycie programu [Visual Studio] lub [SQL Server Management Studio].

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a>Tworzenie bazy danych w usłudze SQL Data Warehouse

Magazyn danych SQL Data Warehouse to specjalny typ bazy danych zaprojektowany z myślą o masowym przetwarzaniu równoległym. Baza danych jest rozpowszechniana w wielu węzłach i przetwarza zapytania równolegle. Magazyn danych SQL Data Warehouse zawiera węzeł kontrolny, który organizuje działania wszystkich węzłów. Węzły zarządzają danymi przy użyciu bazy danych SQL Database.  

> [!NOTE]
> Utworzenie bazy danych w usłudze SQL Data Warehouse może skutkować powstaniem nowej usługi płatnej.  Aby uzyskać więcej informacji, zobacz [Cennik usługi SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>

### <a name="create-a-data-warehouse"></a>Tworzenie magazynu danych

1. Zaloguj się do [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **Nowy** > **Bazy danych** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)
    ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Wypełnij szczegóły dotyczące wdrożenia

    **Nazwa bazy danych**: wybierz dowolną. Jeśli istnieje wiele magazynów danych, zaleca się, aby nazwy zawierały takie informacje jak region czy środowisko, na przykład *mójmagazyndanych-zachodniestanyzjednoczone-1-test*.

    **Subskrypcja**: Twoja subskrypcja platformy Azure

    **Grupa zasobów**: utwórz grupę zasobów lub użyj istniejącej grupy zasobów.
    > [!NOTE]
    > Grupy zasobów są przydatne w przypadku administrowania zasobami, co obejmuje na przykład wyznaczanie zakresu kontroli dostępu i wdrażanie oparte na szablonach. Więcej o grupach zasobów platformy Azure i najlepszych praktykach możesz dowiedzieć się [tutaj](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)

    **Źródło**: pusta baza danych

    **Serwer**: wybierz serwer utworzony w sekcji [Wymagania wstępne].

    **Sortowanie**: pozostaw sortowanie domyślne SQL_Latin1_General_CP1_CI_AS.

    **Wydajność instrukcji SELECT**: zalecamy pozostawienie standardowej wartości 400 jednostek DWU.

4. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**
    ![Przypnij do pulpitu nawigacyjnego](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)

5. Zaczekaj na wdrożenie magazynu danych. Zwykle ten proces trwa kilka minut. W portalu zostanie wyświetlone powiadomienie, gdy magazyn danych będzie gotowy do użycia. 

## <a name="connect-to-sql-data-warehouse"></a>Nawiązywanie połączenia z usługą SQL Data Warehouse

W tym samouczku do nawiązania połączenia z magazynem danych jest używany program SQL Server Management Studio (SSMS). Możesz nawiązać połączenie z usługą SQL Data Warehouse za pomocą następujących obsługiwanych łączników: ADO.NET, JDBC, ODBC i PHP. W przypadku narzędzi nieobsługiwanych przez firmę Microsoft funkcjonalność może być ograniczona.


### <a name="get-connection-information"></a>Pobieranie informacji o połączeniu

Aby nawiązać połączenie z magazynem danych, musisz nawiązać połączenie za pośrednictwem serwera logicznego SQL utworzonego w sekcji [Wymagania wstępne].

1. Wybierz magazyn danych z pulpitu nawigacyjnego lub wyszukaj go w swoich zasobach.

    ![Pulpit nawigacyjny usługi SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Znajdź pełną nazwę serwera logicznego SQL.

    ![Wybieranie nazwy serwera](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Otwórz program SSMS i użyj Eksploratora obiektów do nawiązania połączenia z tym serwerem za pomocą poświadczeń administratora serwera utworzonych w sekcji [Wymagania wstępne]

    ![Nawiązywanie połączenia z programem SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Jeśli wszystko przebiegnie prawidłowo, połączenie z serwerem logicznym SQL powinno być aktywne. Logujesz się jako administrator serwera, dlatego możesz nawiązać połączenie z dowolną bazą danych hostowaną przez serwer, łącznie z bazą danych master. 

Jest tylko jedno konto administratora serwera. Ma ono największe uprawnienia spośród wszystkich użytkowników. Nie podawaj hasła administratora zbyt wielu osobom w organizacji. 

Możesz również mieć konto administratora usługi Azure Active Directory. Nie przedstawiamy tutaj szczegółowych informacji na ten temat. Aby dowiedzieć się więcej o używaniu uwierzytelniania usługi Azure Active Directory, zobacz [Uwierzytelnianie za pomocą usługi Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

Następnym tematem jest tworzenie dodatkowych kont logowania i użytkowników.


## <a name="create-a-database-user"></a>Tworzenie użytkownika bazy danych

W tym kroku opisano tworzenie konta użytkownika umożliwiającego uzyskiwanie dostępu do magazynu danych. Ponadto pokazano, jak umożliwić użytkownikowi wykonywanie zapytań przy dużej ilości pamięci i zasobów procesora CPU.

### <a name="notes-about-resource-classes-for-allocating-resources-to-queries"></a>Uwagi dotyczące klas zasobów na potrzeby przydzielania zasobów do zapytań

- Aby zapewnić bezpieczeństwo danych, nie uruchamiaj zapytań w produkcyjnych bazach danych przy użyciu konta administratora serwera. To konto ma największe uprawnienia spośród wszystkich użytkowników i używanie go do wykonywania operacji na danych użytkowników naraża dane na niebezpieczeństwo. Ponadto administrator serwera powinien wykonywać operacje związane z zarządzaniem, dlatego podczas wykonywania operacji ma do dyspozycji małą ilość pamięci i zasobów procesora CPU. 

- Usługa SQL Data Warehouse korzysta ze wstępnie zdefiniowanych ról bazy danych, nazywanych klasami zasobów, które umożliwiają przydzielanie użytkownikom różnej ilości pamięci i zasobów procesora CPU oraz liczby gniazd współbieżności. Każdy użytkownik może należeć do klasy małej, średniej, dużej lub bardzo dużej ilości zasobów. Klasa zasobów użytkownika określa zasoby dostępne dla użytkownika podczas wykonywania zapytań i operacji ładowania.

- W celu uzyskania optymalnej kompresji danych użytkownik zwykle wymaga obciążenia z przydziałem dużej lub bardzo dużej ilości zasobów. Więcej o klasach zasobów może dowiedzieć się [tutaj](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="create-an-account-that-can-control-a-database"></a>Tworzenie konta z możliwością sterowania bazą danych

Obecnie logujesz się jako administrator serwera, dlatego masz uprawnienia do tworzenia kont logowania i użytkowników.

1. Za pomocą programu SSMS lub innego klienta zapytań otwórz nowe zapytanie dotyczące bazy danych **master**.

    ![Nowe zapytanie w bazie danych master Master](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Nowe zapytanie w bazie danych master Master1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. W oknie zapytania uruchom to polecenie T-SQL, aby utworzyć konto logowania o nazwie MedRCLogin i użytkownika o nazwie LoadingUser. To konto logowania może łączyć się z serwerem logicznym SQL.

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. Podczas wykonywania zapytania w *bazie danych usługi SQL Data Warehouse* utwórz użytkownika bazy danych na podstawie konta logowania utworzonego w celu uzyskania dostępu do bazy danych i wykonywania w niej operacji.

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. Udziel użytkownikowi bazy danych uprawnień do sterowania bazą danych o nazwie NYT. 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] to LoadingUser;
    ```
    > [!NOTE]
    > Jeśli nazwa bazy danych zawiera łączniki, pamiętaj, aby ująć je w nawiasy kwadratowe. 
    >

### <a name="give-the-user-medium-resource-allocations"></a>Przydzielanie użytkownikowi średniej ilości zasobów

1. Uruchom to polecenie T-SQL, aby dodać użytkownika do klasy średniej ilości zasobów o nazwie mediumrc. 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > Kliknij [tutaj](sql-data-warehouse-develop-concurrency.md#resource-classes), aby dowiedzieć się więcej o współbieżności i klasach zasobów. 
    >

2. Nawiązywanie połączenia z serwerem logicznym przy użyciu nowych poświadczeń

    ![Zaloguj się za pomocą nowego identyfikatora logowania](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a>Ładowanie danych usługi Azure Blob Storage

Teraz możesz zacząć ładować dane do magazynu danych. W tym kroku pokazano, jak załadować dane taksówek w Nowym Jorku z obiektu blob w publicznym magazynie platformy Azure. 

- Typową metodą ładowania danych do usługi SQL Data Warehouse jest przeniesienie danych do usługi Azure Blob Storage, a następnie załadowanie ich do magazynu danych. W celu łatwiejszego zrozumienia sposobu ładowania dane nowojorskich taksówek są już hostowane w obiekcie blob w publicznym magazynie platformy Azure. 

- Aby dowiedzieć się, jak przesłać dane do usługi Azure Blob Storage lub załadować je bezpośrednio ze źródła do usługi SQL Data Warehouse, zobacz [omówienie ładowania](sql-data-warehouse-overview-load.md).


### <a name="define-external-data"></a>Definiowanie danych zewnętrznych

1. Utwórz klucz główny. Musisz utworzyć klucz główny tylko raz dla danej bazy danych. 

    ```sql
    CREATE MASTER KEY;
    ```

2. Zdefiniuj lokalizację obiektu blob platformy Azure zawierającego dane taksówek.  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. Zdefiniuj formaty plików zewnętrznych

    Polecenie ```CREATE EXTERNAL FILE FORMAT``` służy do określania formatu plików zawierających dane zewnętrzne. Zawierają one tekst rozdzielony znakami nazywanymi ogranicznikami. Do celów pokazu dane taksówek są przechowywane jako nieskompresowane dane i skompresowane dane w formacie gzip.

    Uruchom te polecenia T-SQL, aby zdefiniować dwa różne formaty: nieskompresowany i skompresowany.

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  Utwórz schemat dla formatu plików zewnętrznych. 

    ```sql
    CREATE SCHEMA ext;
    ```
5. Utwórz tabele zewnętrzne. Te tabele odwołują się do danych przechowywanych w usłudze Azure Blob Storage. Uruchom poniższe polecenia T-SQL, aby utworzyć tabele zewnętrzne wskazujące obiekt blob platformy Azure zdefiniowany wcześniej w naszym zewnętrznym źródle danych.

```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    ```

### Import the data from Azure blob storage.

SQL Data Warehouse supports a key statement called CREATE TABLE AS SELECT (CTAS). This statement creates a new table based on the results of a select statement. The new table has the same columns and data types as the results of the select statement.  This is an elegant way to import data from Azure blob storage into SQL Data Warehouse.

1. Run this script to import your data.

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. View your data as it loads.

   You’re loading several GBs of data and compressing it into highly performant clustered columnstore indexes. Run the following query that uses a dynamic management views (DMVs) to show the status of the load. After starting the query, grab a coffee and a snack while SQL Data Warehouse does some heavy lifting.
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. View all system queries.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Enjoy seeing your data nicely loaded into your Azure SQL Data Warehouse.

    ![See Data Loaded](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## Improve query performance

There are several ways to improve query performance and to achieve the high-speed performance that SQL Data Warehouse is designed to provide.  

### See the effect of scaling on query performance 

One way to improve query performance is to scale resources by changing the DWU service level for your data warehouse. Each service level costs more, but you can scale back or pause resources at any time. 

In this step, you compare performance at two different DWU settings.

First, let's scale the sizing down to 100 DWU so we can get an idea of how one compute node might perform on its own.

1. Go to the portal and select your SQL Data Warehouse.

2. Select scale in the SQL Data Warehouse blade. 

    ![Scale DW From portal](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Scale down the performance bar to 100 DWU and hit save.

    ![Scale and save](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Wait for your scale operation to finish.

    > [!NOTE]
    > Queries cannot run while changing the scale. Scaling **kills** your currently running queries. You can restart them when the operation is finished.
    >
    
5. Do a scan operation on the trip data, selecting the top million entries for all the columns. If you're eager to move on quickly, feel free to select fewer rows. Take note of the time it takes to run this operation.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. Scale your data warehouse back to 400 DWU. Remember, each 100 DWU is adding another compute node to your Azure SQL Data Warehouse.

7. Run the query again! You should notice a significant difference. 

> [!NOTE]
> Since SQL Data Warehouse uses massively parallel processing. Queries that scan or perform analytic functions on millions of rows experience the true power of
> Azure SQL Data Warehouse.
>

### See the effect of statistics on query performance

1. Run a query that joins the Date table with the Trip table

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount],
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    This query takes a while because SQL Data Warehouse has to shuffle data before it can perform the join. Joins do not have to shuffle data if they are designed to join data in the same way it is distributed. That's a deeper subject. 

2. Statistics make a difference. 
3. Run this statement to create statistics on the join columns.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > SQL DW does not automatically manage statistics for you. Statistics are important for query
    > performance and it is highly recommended you create and update statistics.
    > 
    > **You gain the most benefit by having statistics on columns involved in joins, columns
    > used in the WHERE clause and columns found in GROUP BY.**
    >

3. Run the query from Prerequisites again and observe any performance differences. While the differences in query performance will not be as drastic as scaling up, you should notice a  speed-up. 

## Next steps

You're now ready to query and explore. Check out our best practices or tips.

If you're done exploring for the day, make sure to pause your instance! In production, you can experience enormous 
savings by pausing and scaling to meet your business needs.

![Pause](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## Useful readings

[Concurrency and Workload Management][]

[Best practices for Azure SQL Data Warehouse][]

[Query Monitoring][]

[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse][]

[Migrating Data to Azure SQL Data Warehouse][]

[Concurrency and Workload Management]: sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example
[Best practices for Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Query Monitoring]: sql-data-warehouse-manage-monitor.md
[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Migrating Data to Azure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Prerequisites]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx



<!--HONumber=Feb17_HO1-->


