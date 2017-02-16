---
title: "Azure SQL Data Warehouse — samouczek z wprowadzeniem | Microsoft Docs"
description: "Samouczek z wprowadzeniem do usługi Azure SQL Data Warehouse."
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
ms.date: 12/21/2016
ms.author: elbutter
translationtype: Human Translation
ms.sourcegitcommit: 5bb75bf36892c737568b8129b30bb30b02d01bf2
ms.openlocfilehash: 1227903652a944d9d144917922fe36a4f149f820


---
# <a name="get-started-with-sql-data-warehouse"></a>Rozpoczynanie pracy z usługą SQL Data Warehouse

Samouczek z wprowadzeniem do usługi Azure SQL Data Warehouse. W tym samouczku znajdują się podstawy aprowizacji i ładowania danych do usługi SQL Data Warehouse, a także pewne podstawowe informacje dotyczące skalowania, wstrzymywania i dostrajania. 

**Szacowany czas trwania:** 75 minut

## <a name="prerequisites"></a>Wymagania wstępne


### <a name="sign-up-for-microsoft-azure"></a>Utwórz konto na platformie Microsoft Azure
Jeśli nie masz jeszcze konta platformy Microsoft Azure, musisz utworzyć takie konto, aby móc używać tej usługi. Jeśli masz już konto platformy Azure, pomiń ten krok. 

1. Przejdź do strony konta [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)
2. Utwórz bezpłatne konto platformy Azure lub zakup konto.
3. Postępuj zgodnie z instrukcjami

### <a name="install-appropriate-sql-client-driver-and-tools"></a>Zainstaluj odpowiedni sterownik i narzędzia klienta SQL

Większość narzędzi klienta SQL może nawiązywać połączenie z usługą Azure SQL Data Warehouse przy użyciu sterownika JDBC, ODBC lub ADO.net. Z powodu złożoności produktu oraz dużej liczby funkcji w języku T-SQL obsługiwanych przez usługę SQL Data Warehouse, nie każda aplikacja kliencka będzie w pełni zgodna z usługą SQL Data Warehouse.

Jeśli używasz systemu operacyjnego Windows, zalecamy użycie programu [Visual Studio] lub [SQL Server Management Studio].


[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-an-azure-sql-data-warehouse"></a>Tworzenie bazy danych w usłudze Azure SQL Data Warehouse

> [!NOTE]
> Utworzenie bazy danych w usłudze SQL Data Warehouse może skutkować powstaniem nowej usługi płatnej.  Aby uzyskać więcej informacji, zobacz [Cennik usługi SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>


### <a name="create-a-sql-data-warehouse"></a>Tworzenie bazy danych w usłudze SQL Data Warehouse
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **Nowy** > **Bazy danych** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)
    ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Wypełnij szczegóły dotyczące wdrożenia

    **Nazwa bazy danych**: wybierz dowolną. Jeśli istnieje wiele wystąpień magazynu danych SQL Data Warehouse, zaleca się, aby nazwa zawierała szczegóły, takie jak region, środowisko, itp., na przykład *mójmagazyndanych-zachodniestanyzjednoczone-1-test*

    **Subskrypcja**: subskrypcja usługi Azure użytkownika

    **Grupa zasobów**: utwórz nową lub użyj istniejącej, jeśli planowane jest używanie usługi SQL Data Warehouse z innymi usługami
    > [!NOTE]
    > Usługi w ramach grupy zasobów powinny mieć taki sam cykl życia. Grupy zasobów są przydatne w przypadku administrowania zasobami, co obejmuje na przykład wyznaczanie zakresu kontroli dostępu i wdrażanie oparte na szablonach. Więcej o grupach zasobów platformy Azure i najlepszych praktykach możesz dowiedzieć się [tutaj](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
    >

    **Źródło**: pusta baza danych

    **Serwer**: wybierz serwer utworzony w sekcji [Wymagania wstępne].

    **Sortowanie**: pozostaw sortowanie domyślne SQL_Latin1_General_CP1_CI_AS

    **Wydajność instrukcji SELECT**: zalecamy pozostawienie standardowej wartości 400 jednostek DWU

4. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**
    ![Przypnij do pulpitu nawigacyjnego](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)

5. Zaczekaj na wdrożenie usługi Azure SQL Data Warehouse. Zwykle ten proces trwa kilka minut. W portalu zostanie wyświetlone powiadomienie, gdy wdrażanie wystąpienia zostanie ukończone. 

## <a name="connect-to-azure-sql-data-warehouse-through-sql-server-logical-server"></a>Nawiązywanie połączenia z usługą SQL Data Warehouse za pomocą programu SQL Server (serwer logiczny)

W tym samouczku do nawiązania połączenia z usługą SQL Data Warehouse używany jest program SQL Server Management Studio. Możliwe jest użycie innych narzędzi do nawiązania połączenia z usługą SQL Data Warehouse za pomocą obsługiwanych łączników: ADO.NET, JDBC, ODBC i PHP. Należy pamiętać, że dla narzędzi nieobsługiwanych przez firmę Microsoft funkcjonalność może być ograniczona.


### <a name="get-connection-information"></a>Pobieranie informacji o połączeniu

Aby nawiązać połączenie z usługą SQL Data Warehouse, należy nawiązać połączenie za pośrednictwem serwera SQL Server (serwer logiczny) utworzonego w sekcji [Wymagania wstępne].

1. Wybierz magazyn danych SQL Data Warehouse z pulpitu nawigacyjnego lub wyszukaj go w swoich zasobach.

    ![Pulpit nawigacyjny usługi SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Znajdź pełną nazwę dla serwera logicznego.

    ![Wybieranie nazwy serwera](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Otwórz program SSMS i użyj Eksploratora obiektów do nawiązania połączenia z tym serwerem za pomocą poświadczeń utworzonych w sekcji [Wymagania wstępne]

    ![Nawiązywanie połączenia z programem SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Jeśli wszystko przebiegnie prawidłowo, w tym momencie użytkownik powinien być połączony z wystąpieniem serwera SQL Server (serwer logiczny). Można użyć poświadczeń serwera w celu uwierzytelniania w dowolnej bazie danych na serwerze jako jej właściciel. Jako najlepszą praktykę zaleca się jednak, aby utworzyć osobne identyfikatory logowania i użytkowników dla każdej bazy danych. Więcej informacji dotyczących tworzenia użytkowników można znaleźć w sekcji [Tworzenie użytkownika na potrzeby usługi SQL Data Warehouse](./sql-data-warehouse-get-started-tutorial.md#create-a-user-for-sql-data-warehouse). 

## <a name="create-a-user-for-sql-data-warehouse"></a>Tworzenie użytkownika na potrzeby usługi SQL Data Warehouse

### <a name="why-create-a-separate-user"></a>W jakim celu tworzony jest osobny użytkownik?

Do utworzenia nowego użytkownika na potrzeby usługi SQL Data Warehouse używane jest połączenie z serwerem SQL Server (serwer logiczny) z poświadczeniami serwera z poprzedniego kroku. Istnieją dwie podstawowe przyczyny, dla których warto utworzyć osobnego użytkownika lub identyfikator logowania na potrzeby usługi SQL Data Warehouse.

1.  Użytkownicy w Twojej organizacji powinni używać innego konta do uwierzytelniania. W ten sposób można ograniczyć uprawnienia przyznane aplikacji i zmniejszyć ryzyko złośliwych działań.

2. Domyślnie identyfikator logowania administratora serwera, z którym aktualnie jest nawiązane połączenie, używa mniejszej klasy zasobów. Klasy zasobów ułatwiają sterowania alokacją pamięci i cyklami procesora CPU na potrzeby zapytania. Dla użytkowników przypisanych do klasy **smallrc** jest przyznawana mniejsza ilość pamięci i mogą oni korzystać z wyższej współbieżności. Z kolei użytkownikom przypisanym do klasy **xlargerc** jest przyznawana większa ilość pamięci, w związku z czym mniejsza liczba ich zapytań może działać współbieżnie. Aby ładować dane w sposób najlepiej optymalizujący kompresję, należy zapewnić, że użytkownik ładujący dane należy do większej klasy zasobów. Więcej o klasach zasobów może dowiedzieć się [tutaj](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="creating-a-user-of-a-larger-resource-class"></a>Tworzenie użytkownika należącego do większej klasy zasobów

1. Utwórz nowe zapytanie w bazie danych **master** serwera

    ![Nowe zapytanie w bazie danych master Master](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Nowe zapytanie w bazie danych master Master1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. Utwórz identyfikator logowania oraz użytkownika na serwerze

    ```sql
    CREATE LOGIN XLRCLOGIN WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN XLRCLOGIN;
    ```

3. Utwórz nowego użytkownika bazy danych na podstawie identyfikatora logowania na serwerze
    ```sql
    CREATE USER LoadingUser FOR LOGIN XLRCLOGIN;
    ```

4. Przyznaj użytkownikowi kontrolę nad bazą danych
    ```sql
    GRANT CONTROL ON DATABASE::[NYT] to LoadingUser;
    ```
    > [!NOTE]
    > Jeśli nazwa bazy danych zawiera łączniki, pamiętaj, aby ująć je w nawiasy kwadratowe. 
    >

5. Dodaj użytkownika bazy danych do roli klasy zasobów **xlargerc**
    ```sql
    EXEC sp_addrolememeber 'xlargerc', 'LoadingUser';
    ```

6. Zaloguj się do bazy danych przy użyciu nowych poświadczeń

    ![Zaloguj się za pomocą nowego identyfikatora logowania](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="loading-data"></a>Ładowanie danych

### <a name="defining-external-data"></a>Definiowanie danych zewnętrznych
1. Zdefiniuj zewnętrzne źródło danych

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
    TYPE = Hadoop
    , LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```


2. Zdefiniuj formaty plików zewnętrznych

    Polecenie ```CREATE EXTERNAL FILE FORMAT``` służy do określania formatu danych zewnętrznych, na podstawie których odbywa się ładowanie. Na potrzeby publicznych danych taksówek w Nowym Jorku użyte zostały dwa formaty w celu przechowywania danych w magazynie Azure Blob Storage

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH
    ( FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = ','
    , STRING_DELIMITER = ''
    , DATE_FORMAT = ''
    , USE_TYPE_DEFAULT = False
    )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH
    ( FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = '|'
    , STRING_DELIMITER = ''
    , DATE_FORMAT = ''
    , USE_TYPE_DEFAULT = False
    )
    , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

3.  Utwórz schemat dla formatu plików zewnętrznych

    ```sql
    CREATE SCHEMA ext;
    GO
    ```

4. Utwórz tabele zewnętrzne. Te tabele odwołują się do danych przechowywanych w system plików HDFS lub magazynie Azure Blob Storage. 

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
    LOCATION = 'Date'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
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
    LOCATION = 'Geography'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0 
    )
    ;
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
    [HackneyLicenseID] int NOT NULL,
    [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
    [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
    LOCATION = 'HackneyLicense'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
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
    LOCATION = 'Medallion'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
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
    LOCATION = 'Time'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
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
    LOCATION = 'Trip2013'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = compressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
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
    LOCATION = 'Weather2013'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
    ;
    ```

### <a name="create-table-as-select-ctas"></a>Tworzenie tabeli przy użyciu instrukcji SELECT (CTAS)

5. Załaduj dane z tabel zewnętrznych do wystąpienia usługi SQL Data Warehouse. 
    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    CREATE TABLE [dbo].[Geography]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    CREATE TABLE [dbo].[Medallion]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    CREATE TABLE [dbo].[Time]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    CREATE TABLE [dbo].[Weather]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    CREATE TABLE [dbo].[Trip]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

    > [!NOTE]
    > Ładowane jest kilka gigabajtów danych, które są kompresowane do wysoce wydajnych indeksów magazynu kolumn klastra. Uruchom następujące zapytanie DMV, a następnie poczekaj, aż usługa Azure SQL Data Warehouse wykona całą pracę.
    >

6. Utwórz nowe zapytanie i obserwuj pojawianie się Twoich zdanych w dynamicznym widoku zarządzania (DMV, Dynamic Management View)

    ```sql
    SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files,
    sum(s.bytes_processed)/1024/1024 as gb_processed
    FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
    on r.request_id = s.request_id
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
    nbr_files desc, gb_processed desc;
    ```

7. Wyświetl wszystkie zapytania systemowe

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

8. Korzystaj z danych poprawnie załadowanych do usługi Azure SQL Data Warehouse

    ![Wyświetl załadowane dane](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)



## <a name="querying-data"></a>Wykonywanie zapytań na danych 

### <a name="scan-query-with-scaling"></a>Zapytanie skanowania ze skalowaniem

Dowiedz się, w jaki sposób skalowanie wpływa na szybkość zapytań.

Przed rozpoczęciem przeprowadźmy skalowanie operacji do 100 jednostek DWU, dzięki czemu będzie można poznać wydajność pojedynczego węzła obliczeniowego.

1. Przejdź do portalu i wybierz wystąpienie usługi SQL Data Warehouse

2. Wybierz skalę w bloku SQL Data Warehouse. 

    ![Skalowanie usługi SQL Data Warehouse z poziomu portalu](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Przeprowadź skalowanie w dół na pasku wydajności do 100 jednostek DWU, a następnie kliknij pozycję Zapisz.

    ![Skaluj i zapisz](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Poczekaj na zakończenie operacji skalowania.

    > [!NOTE]
    > Należy pamiętać, że operacje skalowania powodują **skasowanie** aktualnie uruchomionych zapytań i uniemożliwiają uruchomienie nowych.
    >
    
5. Wykonaj operację skanowania danych podróży, zaznaczając pierwszy milion wpisów dla wszystkich kolumn. Jeśli zależy Ci na czasie, możesz wybrać mniej wierszy.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```

Zapisz czas potrzebny do wykonania tej operacji.

6. Przeprowadź skalowanie w górę wystąpienia do 400 jednostek DWU. Pamiętaj, że każde 100 jednostek DWU powoduje dodanie dodatkowego węzła obliczeniowego do magazynu danych Azure SQL Data Warehouse.

7. Uruchom zapytanie ponownie. Powinna być zauważalna istotna różnica. 

> [!NOTE]
> Usługa SQL Data Warehouse to platforma masowego przetwarzania równoległego (MPP, Massively Parallel Processing). Zapytania i operacje umożliwiające zrównoleglanie pracy między różnymi węzłami stanowią największą siłę usługi Azure SQL Data Warehouse.
>

### <a name="join-query-with-statistics"></a>Zapytanie sprzężenia ze statystykami

1. Uruchom zapytanie powodujące sprzężenie tabeli Date z tabelą Trip

    ```sql
    SELECT TOP (1000000) dt.[DayOfWeek]
    ,tr.[MedallionID]
    ,tr.[HackneyLicenseID]
    ,tr.[PickupTimeID]
    ,tr.[DropoffTimeID]
    ,tr.[PickupGeographyID]
    ,tr.[DropoffGeographyID]
    ,tr.[PickupLatitude]
    ,tr.[PickupLongitude]
    ,tr.[PickupLatLong]
    ,tr.[DropoffLatitude]
    ,tr.[DropoffLongitude]
    ,tr.[DropoffLatLong]
    ,tr.[PassengerCount]
    ,tr.[TripDurationSeconds]
    ,tr.[TripDistanceMiles]
    ,tr.[PaymentType]
    ,tr.[FareAmount]
    ,tr.[SurchargeAmount]
    ,tr.[TaxAmount]
    ,tr.[TipAmount]
    ,tr.[TollsAmount]
    ,tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
    join
    dbo.[Date] as dt
    on tr.DateID = dt.DateID
    ```

    Zgodnie z oczekiwaniami zapytanie zabiera znacznie więcej czasu w przypadku mieszania danych między węzłami, szczególnie w przypadku tego typu scenariusza sprzężenia.

2. Sprawdźmy, jaka będzie różnica, gdy statystyki dla sprzężonej kolumny zostaną utworzone w następujący sposób:

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > Usługa SQL Data Warehouse nie zarządza statystykami automatycznie. Statystyki są istotne w przypadku wydajności zapytań i zdecydowanie zalecane jest ich tworzenie i aktualizowanie.
    > 
    > **Największe korzyści można osiągnąć, prowadząc statystyki dla kolumn uczestniczących w sprzężeniach, kolumn używanych w ramach klauzuli WHERE i kolumn z klauzuli GROUP BY.**
    >

3. Uruchom zapytanie ponownie z poziomu sekcji Wymagania wstępne i obserwuj różnice wydajności. Mimo że różnice w wydajności zapytania nie będą tak znaczące, jak w przypadku skalowania w górę, przyspieszenie powinno być odczuwalne. 

## <a name="next-steps"></a>Następne kroki

Wszystko jest teraz gotowe do tworzenia zapytań i eksplorowania. Zapoznaj się z naszymi najlepszymi praktykami i poradami.

Po zakończeniu eksplorowania pamiętaj, aby wstrzymać wystąpienie. W środowisku produkcyjnym można poczynić ogromne oszczędności dzięki wstrzymywaniu i skalowaniu w celu osiągnięcia własnych potrzeb biznesowych.

![Wstrzymaj](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a>Przydatne zasoby

[Współbieżność i zarządzanie obciążeniami]

[Najlepsze praktyki dotyczące korzystania z usługi Azure SQL Data Warehouse]

[Monitorowanie zapytań]

[10 najlepszych praktyk dotyczących tworzenia relacyjnego magazynu danych w dużej skali]

[Migrowanie danych do usługi Azure SQL Data Warehouse]


[Współbieżność i zarządzanie obciążeniami]: sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example
[Najlepsze praktyki dotyczące korzystania z usługi Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Monitorowanie zapytań]: sql-data-warehouse-manage-monitor.md
[10 najlepszych praktyk dotyczących tworzenia relacyjnego magazynu danych w dużej skali]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Migrowanie danych do usługi Azure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Wymagania wstępne]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx



<!--HONumber=Jan17_HO2-->


