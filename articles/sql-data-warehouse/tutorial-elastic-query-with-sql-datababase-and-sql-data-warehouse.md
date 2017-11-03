---
title: "Samouczek elastycznej zapytania z usługi Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak elastycznej zapytania za pomocą usługi Azure SQL Data Warehouse "
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 09/18/2017
ms.author: elbutter
ms.openlocfilehash: 8698dace1b7308fc60178d97e134cb708ff02255
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-elastic-query-with-sql-data-warehouse"></a>Konfigurowanie elastycznej zapytania przy użyciu magazynu danych SQL

Z tego samouczka dowiesz się, jak używać elastycznej zapytania do przesyłania kwerend z bazy danych SQL do usługi SQL Data Warehouse. Zapytanie elastycznej jest funkcje, które istnieje między produktów Azure SQL. Aby uzyskać więcej informacji o elastycznej zapytania jako koncepcji, zobacz [ **sposobu używania elastycznej zapytania z usługi SQL Data Warehouse**][How to use Elastic Query with SQL Data Warehouse].

## <a name="prerequisites-for-the-tutorial"></a>Wymagania wstępne dotyczące samouczka

Przed rozpoczęciem tego samouczka wymagane są następujące wymagania wstępne:

1. Zainstalowany program SQL Server Management Studio (SSMS).
2. Utworzyć Azure SQL server z magazynem danych i bazy danych w ramach tego serwera.
3. Konfiguracja reguł zapory na potrzeby uzyskiwania dostępu do serwera SQL Azure.

## <a name="set-up-connection-between-sql-data-warehouse-and-sql-database-instances"></a>Skonfiguruj połączenie między wystąpieniami usługi SQL Data Warehouse i bazy danych SQL 

1. Przy użyciu narzędzia SSMS lub innego zapytania klienta, Otwórz nowe zapytanie bazy danych **wzorca** na serwerze w sieci logicznej.

2. Utwórz nazwę logowania i użytkownika, który reprezentuje bazy danych SQL do połączenia magazynu danych.

  ```sql
  CREATE LOGIN SalesDBLogin WITH PASSWORD = 'aReallyStrongPassword!@#';
  ```

3. Przy użyciu narzędzia SSMS lub innego zapytania klienta, Otwórz nowe zapytanie dla **wystąpienie magazynu danych SQL** na serwerze w sieci logicznej.

4. Utwórz użytkownika w wystąpieniu magazynu danych z nazwą logowania, który został utworzony w kroku 2

   ```sql
   CREATE USER SalesDBUser FOR LOGIN SalesDBLogin;
   ```

5. Przyznaj uprawnienia do użytkownika z kroku 4, który chcesz bazy danych SQL chcesz wykonać. W tym przykładzie uprawnienie jest tylko udzielenia wybierz pozycję określonego schematu, pokazujący, jak firma Microsoft może ograniczać kwerend z bazy danych SQL do określonej domeny. 

   ```sql
   GRANT SELECT ON SCHEMA :: [dbo] TO SalesDBUser;
   ```

6. Przy użyciu narzędzia SSMS lub innego zapytania klienta, Otwórz nowe zapytanie dla **wystąpienia bazy danych SQL** na serwerze w sieci logicznej.

7. Utwórz klucz główny, jeśli użytkownik nie ma jeszcze jednej. 

   ```sql
   CREATE MASTER KEY; 
   ```

8. Utwórz poświadczenia bazy danych przy użyciu poświadczeń, który został utworzony w kroku 2.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL SalesDBElasticCredential
   WITH IDENTITY = 'SalesDBLogin',
   SECRET = 'aReallyStrongPassword@#!';
   ```

9. Tworzenie zewnętrznego źródła danych wskazujące wystąpienie magazynu danych.

  ```sql
  CREATE EXTERNAL DATA SOURCE EnterpriseDwSrc WITH 
      (TYPE = RDBMS, 
      LOCATION = '<SERVER NAME>.database.windows.net', 
      DATABASE_NAME = '<SQL DATA WAREHOUSE NAME>', 
      CREDENTIAL = SalesDBElasticCredential, 
  ) ;
  ```

10. Teraz można tworzyć tabel zewnętrznych, które odwołują się do tego źródła danych zewnętrznych. Zapytania przy użyciu tych tabel są wysyłane do wystąpienie magazynu danych do przetwarzania i wysyłane z powrotem do tego wystąpienia bazy danych.


## <a name="elastic-query-from-sql-database-to-sql-data-warehouse"></a>Elastyczne zapytania z bazy danych SQL do magazynu danych SQL

W następnych kilku krokach utworzymy tabelę w naszym wystąpienie magazynu danych z wielu wartości. Następnie zostanie przedstawiony sposób skonfigurować tabelę zewnętrzną do badania wystąpienie magazynu danych z tego wystąpienia bazy danych.

1. Przy użyciu narzędzia SSMS lub innego zapytania klienta, Otwórz nowe zapytanie dla **SQL Data Warehouse** na serwerze w sieci logicznej.

2. Przedstawia następujące zapytanie w celu utworzenia **OrdersInformation** tabeli w wystąpieniu magazynu danych.

   ```sql
   CREATE TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL 
   ,   [CustomerID] [int] NOT NULL 
   ) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)
   ```

3. Przy użyciu narzędzia SSMS lub innego zapytania klienta, Otwórz nowe zapytanie dla **bazy danych SQL** na serwerze w sieci logicznej.

4. Przedstawia następujące zapytanie w celu utworzenia definicji tabeli zewnętrznej, który wskazuje **OrdersInformation** tabeli w wystąpieniu magazynu danych.

   ```sql
   CREATE EXTERNAL TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL
   ,   [CustomerID] [int] NOT NULL 
   ) 
   WITH 
   (DATA_SOURCE = EnterpriseDwSrc)
   ```

5. Sprawdź, czy masz teraz definicji tabeli zewnętrznej Twojej **wystąpienia bazy danych SQL**.

   ![Definicja tabeli zewnętrznej elastycznej zapytania](./media/sql-data-warehouse-elastic-query-with-sql-database/elastic-query-external-table.png)


6. Przedstawia następujące zapytanie, który sprawdza wystąpienie magazynu danych. Powinien zostać wyświetlony pięciu wartości, które zostały wstawione w kroku 2. 

```sql
SELECT * FROM [dbo].[OrderInformation];
```

> [!NOTE]
>
> Zwróć uwagę, że mimo kilku wartości, to zapytanie zajmuje wiele czasu do zwrócenia. Jeśli elastycznej zapytania z magazynu danych, co należy rozważyć kosztów przetwarzania zapytań i przesuwanie przez sieć. Priorytet jest moc obliczeniową, a nie czas oczekiwania, korzystać z zapytania elastycznej zdalne wykonywanie kodu.

Gratulacje, po skonfigurowaniu podstawy bardzo elastyczne zapytania. 




<!--Image references-->

<!--Article references-->

[How to use Elastic Query with SQL Data Warehouse]: ./how-to-use-elastic-query-with-sql-data-warehouse.md

<!--MSDN references-->

<!--Other Web references-->