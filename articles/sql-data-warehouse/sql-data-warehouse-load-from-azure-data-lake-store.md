---
title: "Obciążenie — usługi Azure Data Lake Store SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak ładowanie danych z usługi Azure Data Lake Store do usługi Azure SQL Data Warehouse przy użyciu programu PolyBase tabel zewnętrznych."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 09/15/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: bb478484fba5a76fa12d5d1976919224965b6e0d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a>Ładowanie danych z usługi Azure Data Lake Store do magazynu danych SQL
Ten dokument stanowi wszystkie czynności, które należy załadować własne dane z usługi Azure Data Lake magazyn (ADLS) do usługi SQL Data Warehouse przy użyciu programu PolyBase.
Gdy jesteś w stanie uruchamianie zapytań ad hoc przez dane przechowywane w ADLS przy użyciu tabel zewnętrznych, jako najlepsze rozwiązanie zaleca się importowania danych do usługi SQL Data Warehouse.

W tym samouczku przedstawiono sposób:

1. Tworzenie obiektów zewnętrznej bazy danych można załadować z usługi Azure Data Lake Store.
2. Nawiązać katalog usługi Azure Data Lake Store.
3. Ładowanie danych do usługi Azure SQL Data Warehouse.

## <a name="before-you-begin"></a>Przed rozpoczęciem
Aby uruchomić ten samouczek, potrzebne są:

* Azure aplikacji usługi Active Directory do użycia na potrzeby uwierzytelniania do usługi. Aby utworzyć, wykonaj [uwierzytelniania usługi Active directory](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)

>[!NOTE] 
> Potrzebujesz Identyfikatora klienta, klucz i wartość tokenu punktu końcowego OAuth2.0 aplikacji Active Directory do nawiązania połączenia z usługi Azure Data Lake z magazynu danych SQL. Szczegóły dotyczące sposobu uzyskania tych wartości znajdują się w łącze powyżej.
>Uwaga dotycząca rejestracji aplikacji Azure Active Directory, użyj Identyfikatora aplikacji jako identyfikator klienta.

* SQL Server Management Studio lub SQL Server Data Tools, aby pobrać narzędzia SSMS i połączyć zobacz [SSMS zapytania](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)

* Magazyn danych SQL Azure, aby utworzyć wykonaj jedną: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision

* Azure Data Lake Store, lub nie jest włączone szyfrowanie. Aby utworzyć wykonaj jedną: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal




## <a name="configure-the-data-source"></a>Konfigurowanie źródła danych
Aparat PolyBase używa obiektów zewnętrznych T-SQL w celu zdefiniowania lokalizacji i atrybuty danych zewnętrznych. Obiekty zewnętrzne są przechowywane w usłudze SQL Data Warehouse i odwołują się dane, które th są przechowywane zewnętrznie.


###  <a name="create-a-credential"></a>Utwórz poświadczenia
Aby uzyskać dostęp do usługi Azure Data Lake Store, należy utworzyć klucz główny bazy danych, aby zaszyfrować klucz tajny poświadczenie użyte w następnym kroku.
Następnie można utworzyć poświadczenia bazy danych, której są przechowywane poświadczenia główne usługi skonfigurowane w usłudze AAD. Dla osób, które użyto który można podłączyć do obiektów blob magazynu Azure z systemem Windows, należy pamiętać, że składnia poświadczeń różnych PolyBase.
Aby połączyć się z usługi Azure Data Lake Store, należy najpierw **pierwszy** tworzenie aplikacji Azure Active Directory Utwórz klucz dostępu i umożliwić aplikacji dostęp do zasobów usługi Azure Data Lake. Instrukcje, aby wykonać te czynności znajdują się [tutaj](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://msdn.microsoft.com/en-us/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/en-us/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- It should look something like this:
CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```


### <a name="create-the-external-data-source"></a>Tworzenie zewnętrznego źródła danych
Użyj tej [Tworzenie zewnętrznego źródła danych] [ CREATE EXTERNAL DATA SOURCE] polecenia do przechowywania lokalizacji danych i typu danych. Aby znaleźć identyfikator URI ADL w portalu Azure, przejdź do usługi Azure Data Lake Store i przyjrzyj się panelu Essentials.

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Store.
-- LOCATION: Provide Azure Data Lake accountname and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalakestore.net',
    CREDENTIAL = ADLCredential
);
```



## <a name="configure-data-format"></a>Skonfiguruj format danych
Aby zaimportować dane z ADLS, należy określić format pliku zewnętrznego. To polecenie ma format opcje do opisywania danych.
Poniżej przedstawiono przykład często używane formacie, który jest plikiem tekstowym rozdzielany potoku.
Szukaj w naszej dokumentacji T-SQL, aby uzyskać pełną listę [Tworzenie zewnętrznych FORMAT pliku][CREATE EXTERNAL FILE FORMAT]

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store all Missing values as NULL

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-external-tables"></a>Tworzenie tabel zewnętrznych
Teraz, gdy został określony format źródła i plików danych, możesz przystąpić do tworzenia tabel zewnętrznych. Tabele zewnętrzne są interakcje z danymi zewnętrznymi. Program PolyBase używa cyklicznego katalogu Przechodzenie do odczytu wszystkich plików w podkatalogach katalogu określonego w parametrze lokalizacji. Ponadto poniższy przykład pokazuje, jak utworzyć obiektu. Należy dostosować instrukcję do pracy z danymi w ADLS.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the ADLS root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStore
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Zagadnienia dotyczące tabeli zewnętrznej
Tworzenie tabeli zewnętrznej jest proste, ale istnieją pewne różnice, które muszą zostać omówione.

Ładowanie danych przy użyciu programu PolyBase jest silnie typizowane. Oznacza to, że każdy wiersz danych jest pozyskanych muszą spełniać definicja schematu tabeli.
Jeśli dany wiersz jest niezgodny z definicji schematu, wiersz został odrzucony z obciążenia.

Opcje REJECT_TYPE i REJECT_VALUE umożliwiają definiowanie, ile wierszy lub wartość procentowa danych musi być obecny w końcowym tabeli.
Podczas ładowania po osiągnięciu wartości Odrzuć obciążenia kończy się niepowodzeniem. Najczęstszą przyczyną odrzuconych wierszy jest niezgodność definicji schematu.
Na przykład kolumny niepoprawnie podano schematu int, gdy dane w pliku jest ciągiem, każdy wiersz zakończy się niepowodzeniem do załadowania.

Lokalizacja określa katalogu najwyższego poziomu, który chcesz odczytać danych z.
W takim przypadku, gdyby podkatalogów w obszarze /DimProduct/ PolyBase jaki są importowane wszystkich danych w podkatalogach. Azure Data Lake store używa kontroli dostępu na podstawie ról (RBAC) w celu kontroli dostępu do danych. Oznacza to, że nazwy głównej usługi musi mieć uprawnienia odczytu do katalogów określonych w parametrze lokalizacji i element podrzędny elementu końcowego katalogów i plików. Dzięki temu PolyBase w celu uwierzytelniania i załadować dane do odczytu. 

## <a name="load-the-data"></a>Ładowanie danych
Aby załadować dane z użycia usługi Azure Data Lake Store [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] instrukcji. Ładowanie z CTAS używa jednoznacznie tabeli zewnętrznej, które utworzono.

CTAS tworzy nową tabelę i wypełnia wyników w instrukcji select. CTAS definiuje nowa tabela na tej samej kolumny i typy danych wyników w instrukcji select. Po wybraniu wszystkich kolumn z tabeli zewnętrznej nowa tabela jest repliką kolumn i typy danych w tabeli zewnętrznej.

W tym przykładzie tworzymy tabeli rozproszonej wyznaczania wartości skrótu o nazwie DimProduct z naszych zewnętrznych DimProduct_external tabeli.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>Optymalizacja magazynu kolumn kompresji
Domyślnie usługa SQL Data Warehouse przechowuje tabeli jako klastrowany indeks magazynu kolumn. Po zakończeniu obciążenia niektóre wiersze danych może nie można skompresować do magazynu kolumn.  Brak z różnych powodów, dlaczego jest to możliwe. Aby dowiedzieć się więcej, zobacz [Zarządzaj indeksami magazynu kolumn][manage columnstore indexes].

Aby zoptymalizować wydajność zapytań i ich kompresji magazynu kolumn po załadowaniu, Odbuduj tabelę, aby wymusić indeksu magazynu kolumn do skompresowania wszystkie wiersze.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

Aby uzyskać więcej informacji na temat zachowania indeksy magazynu kolumn, zobacz [Zarządzaj indeksami magazynu kolumn] [ manage columnstore indexes] artykułu.

## <a name="optimize-statistics"></a>Optymalizacja statystyki
Najlepiej utworzyć statystyki dla pojedynczej kolumny natychmiast po załadowaniu. Dostępne są niektóre opcje wyboru statystyk. Na przykład jeśli tworzenie statystyk pojedynczej kolumny w każdej kolumnie go może zająć dużo czasu odbudować wszystkie statystyki. Jeśli wiesz, że niektóre kolumny nie będą znajdować się w predykatach kwerendy, można pominąć tworzenie statystyk na podstawie tych kolumn.

Jeśli zdecydujesz się tworzenie statystyk pojedynczej kolumny w każdej kolumnie każdej tabeli, można użyć procedury składowanej przykładowy kod `prc_sqldw_create_stats` w [statystyki] [ statistics] artykułu.

Poniższy przykład jest dobry punkt wyjścia do tworzenia statystyk. Tworzy statystyki pojedynczej kolumny w każdej kolumnie w tabeli wymiarów i w każdej kolumnie łącząca w tabelach faktów. Można dodać jednego lub wielu kolumn statystyki do kolumn tabeli faktów później.


## <a name="achievement-unlocked"></a>Osiągnięcia odblokowane!
Dane zostały pomyślnie załadowane do magazynu danych SQL Azure. Dobra robota!

## <a name="next-steps"></a>Następne kroki
Podczas ładowania danych jest pierwszy krok projektowania rozwiązania magazynu danych przy użyciu usługi SQL Data Warehouse. Zobacz nasze zasoby projektowe na [tabel](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) i [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).


<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
