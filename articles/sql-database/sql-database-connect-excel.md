---
title: "Łączenie programu Excel z bazą danych SQL Database | Microsoft Docs"
description: "Dowiedz się, jak połączyć program Microsoft Excel z bazą danych Azure SQL w chmurze. Importowanie danych do programu Excel, raportowanie i eksploracja danych."
services: sql-database
keywords: "łączenie programu excel z sql, importowanie danych do programu excel"
documentationcenter: 
author: joseidz
manager: jhubbard
editor: 
ms.assetid: 906924bc-2707-48d3-bac6-397976a0409d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jhubbard
ms.openlocfilehash: 34ff5c479cfcf1e861a82205eef93dfee01cb4a2
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Łączenie programu Excel z bazą danych Azure SQL i utworzyć raport

Łączenie programu Excel z bazą danych SQL w chmurze i importowanie danych i tworzenie tabel i wykresów na podstawie wartości w bazie danych. W tym samouczku skonfigurujesz połączenie między programem Excel i tabelą bazy danych, zapiszesz plik przechowujący dane oraz informacje o połączeniu dla programu Excel, a następnie utworzysz wykres przestawny z wartościami bazy danych.

Przed rozpoczęciem pracy będziesz potrzebować bazy danych SQL na platformie Azure. Jeśli jej nie masz, zobacz artykuł [Create your first SQL database](sql-database-get-started-portal.md) (Tworzenie pierwszej bazy danych SQL), aby w ciągu kilku minut uzyskać gotową do pracy bazę danych z przykładowymi danymi. W tym artykule będzie importować przykładowe dane do programu Excel z tego artykułu, ale można wykonać podobne kroki z użyciem własnych danych.

Potrzebna będzie również kopia programu Excel. W tym artykule wykorzystano program [Microsoft Excel 2016](https://products.office.com/).

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Łączenie programu Excel z bazą danych SQL i tworzenie pliku odc
1. Aby połączyć program Excel z bazą danych SQL, otwórz program Excel, a następnie utwórz nowy skoroszyt lub otwórz istniejący skoroszyt programu Excel.
2. Na pasku menu w górnej części strony kliknij kartę **Dane**, kliknij przycisk **Z innych źródeł**, a następnie kliknij pozycję **Z programu SQL Server**.
   
   ![Wybór źródła danych: połącz program Excel z bazą danych SQL.](./media/sql-database-connect-excel/excel_data_source.png)
   
   Zostanie otwarty Kreator połączenia danych.
3. W oknie dialogowym **Łączenie z serwerem bazy danych** wpisz **nazwę serwera** usługi SQL Database, z którym chcesz nawiązać połączenie w następującej formie: <*nazwaserwera*>**. database.windows.net**. Na przykład **adworkserver.database.windows.net**.
4. W obszarze **Poświadczenia logowania** kliknij opcję **Użyj następującej nazwy użytkownika i hasła**, wpisz **nazwę użytkownika** i **hasło** skonfigurowane dla serwera usługi SQL Database podczas jego tworzenia, a następnie kliknij przycisk **Dalej**.
   
   ![Wpisywanie nazwy serwera i poświadczeń logowania](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > W zależności od środowiska sieciowego może nie być możliwe nawiązanie połączenia lub można utracić połączenie, jeśli serwer usługi SQL Database nie zezwala na ruch z adresu IP klienta. Przejdź do witryny [Azure Portal](https://portal.azure.com/), kliknij serwery SQL, kliknij serwer, którego używasz, kliknij zaporę systemu w ustawieniach i dodaj swój adres IP klienta. Aby uzyskać szczegółowe informacje, zobacz artykuł [How to configure firewall settings](sql-database-configure-firewall-settings.md) (Jak skonfigurować ustawienia zapory).
   > 
   > 
5. W oknie dialogowym **Wybór bazy danych i tabeli** wybierz z listy bazę danych, której chcesz używać, kliknij tabele lub widoki, których chcesz używać (wybrano **vGetAllCategories**), a następnie kliknij przycisk **Dalej**.
   
    ![Wybierz bazę danych i tabelę.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    Zostanie otwarte okno dialogowe **Zapisywanie pliku połączenia danych i kończenie**, w którym należy podać informacje o pliku połączenia bazy danych pakietu Office (*.odc), z którego korzysta program Excel. Można pozostawić wartości domyślne lub dostosować ustawienia.
6. Wartości domyślne możesz pozostawić bez zmian, zwróć jednak uwagę na pole **Nazwa pliku**. **Opis**, **Przyjazna nazwa** i **Słowa kluczowe** przypominają zarówno Tobie, jak i innym użytkownikom, jakie połączenie ma zostać nawiązane, oraz ułatwiają znalezienie połączenia. Kliknij opcję **Zawsze próbuj używać tego pliku w celu odświeżenia danych**, jeśli chcesz, aby informacje o połączeniu były przechowywane w pliku odc i używane w celu aktualizacji danych podczas nawiązywania połączenia, a następnie kliknij przycisk **Zakończ**.
   
    ![Zapisywanie pliku odc](./media/sql-database-connect-excel/save-odc-file.png)
   
    Zostanie otwarte okno dialogowe **Importowanie danych**.

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Importowanie danych do programu Excel i tworzenie wykresu przestawnego
Po nawiązaniu połączenia i utworzeniu pliku zawierającego dane oraz informacje o połączeniu należy zapoznać się z wskazówkami na temat importowania danych.

1. W oknie dialogowym **Importowanie danych** kliknij opcję prezentacji danych w arkuszu, a następnie kliknij przycisk **OK**. Wybrano **Wykres przestawny**. Można również utworzyć **Nowy arkusz** lub wybrać opcję **Dodaj te dane do modelu danych**. Więcej informacji o modelach danych można znaleźć w temacie [Tworzenie modelu danych w programie Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Kliknij przycisk **Właściwości**, aby zapoznać się z informacjami o pliku odc utworzonym w poprzednim kroku i wybrać opcje odświeżania danych.
   
    ![Wybieranie formatu danych w programie Excel](./media/sql-database-connect-excel/import-data.png)
   
    Arkusz zawiera teraz pustą tabelę przestawną i wykres.
2. W obszarze **Pola tabeli przestawnej** zaznacz wszystkie pola wyboru dla pól do wyświetlenia.
   
    ![Skonfiguruj raport bazy danych.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Aby połączyć inne skoroszyty i arkusze programu Excel z bazą danych, kliknij kartę **Dane**, następnie kliknij przycisk **Połączenia** oraz przycisk **Dodaj**, wybierz z listy utworzone połączenie, a następnie kliknij przycisk **Otwórz**.
> ![Otwieranie połączenia z innego skoroszytu](./media/sql-database-connect-excel/open-from-another-workbook.png)
> 
> 

## <a name="next-steps"></a>Następne kroki
* Aby wykonywać zaawansowane zapytania i analizy, zobacz temat [Connect to SQL Database with SQL Server Management Studio](sql-database-connect-query-ssms.md) (Nawiązywanie połączenia z usługą SQL Database za pomocą programu SQL Server Management Studio).
* Dowiedz się, jakie zalety mają [pule elastyczne](sql-database-elastic-pool.md).
* Dowiedz się, jak [utworzyć aplikację sieci Web, która łączy się z bazą danych SQL Database zaplecza](../app-service/app-service-web-tutorial-dotnet-sqldatabase.md).

