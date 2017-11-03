---
title: "Użyj usługi Azure Functions do wykonania zadania oczyszczania bazy danych | Dokumentacja firmy Microsoft"
description: "Użyj usługi Azure Functions można zaplanować zadanie, które łączy się z bazą danych SQL Azure, aby okresowo wyczyszczenia wierszy."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: ac0399867e0cdab1825022c4ed73ce003cc8c7e6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a>Nawiązywanie połączenia z bazy danych SQL Azure za pomocą usługi Azure Functions
W tym temacie przedstawiono sposób użycia usługi Azure Functions można utworzyć zaplanowane zadanie, które utraciły wierszy w tabeli w bazie danych SQL Azure. Nowa funkcja C# jest tworzony na podstawie szablonu wyzwalacza czasomierza wstępnie zdefiniowane w portalu Azure. Aby zapewnić obsługę tego scenariusza, należy także ustawić parametry połączenia bazy danych jako ustawienie w funkcji aplikacji. W tym scenariuszu operacja zbiorcza w bazie danych. Aby funkcja przetworzyć poszczególnych operacji CRUD w tabeli Mobile Apps, zamiast tego należy używać [powiązania Mobile Apps](functions-bindings-mobile-apps.md).

## <a name="prerequisites"></a>Wymagania wstępne

+ W tym temacie funkcja czasomierza wyzwolone. Wykonaj kroki w temacie [Tworzenie funkcji platformy Azure, która jest wyzwalany przez czasomierz](functions-create-scheduled-function.md) tworzenia wersji języka C# tej funkcji.   

+ W tym temacie przedstawiono polecenie języka Transact-SQL, które wykonuje operację czyszczenia zbiorczego w **SalesOrderHeader** tabeli w bazie danych AdventureWorksLT. Aby utworzyć przykładową bazę danych AdventureWorksLT, wykonaj czynności opisane w temacie [tworzenie bazy danych Azure SQL w portalu Azure](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu

Należy pobrać parametry połączenia dla bazy danych utworzonej po wykonaniu [tworzenie bazy danych Azure SQL w portalu Azure](../sql-database/sql-database-get-started-portal.md).

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
 
3. Wybierz **baz danych SQL** z menu po lewej stronie i wybierz bazę danych na **baz danych SQL** strony.

4. Wybierz **Pokaż parametry połączenia bazy danych** i skopiować pełną **ADO.NET** parametry połączenia.

    ![Skopiuj parametry połączenia ADO.NET.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a>Ustawianie parametrów połączenia 

Aplikacja funkcji obsługuje wykonywanie funkcji na platformie Azure. Jest najlepszym rozwiązaniem jest przechowywanie parametrów połączenia i innych informacji poufnych w ustawieniach funkcji aplikacji. Używanie ustawień aplikacji uniemożliwia przypadkowego ujawnienia parametrów połączenia w kodzie. 

1. Przejdź do tworzenia aplikacji funkcji [Tworzenie funkcji platformy Azure, która jest wyzwalany przez czasomierz](functions-create-scheduled-function.md).

2. Wybierz **funkcji platformy** > **ustawienia aplikacji**.
   
    ![Ustawienia aplikacji dla aplikacji funkcja.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Przewiń w dół do **parametry połączenia** i dodaj ciąg połączenia przy użyciu ustawień określonych w tabeli.
   
    ![Dodaj parametry połączenia w ustawieniach aplikacji funkcji.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Ustawienie       | Sugerowana wartość | Opis             | 
    | ------------ | ------------------ | --------------------- | 
    | **Nazwa**  |  sqldb_connection  | Umożliwia dostęp do parametrów połączenia przechowywanych w kodzie funkcji.    |
    | **Wartość** | Ciąg skopiowany  | Poza parametry połączenia skopiowane w poprzedniej sekcji. |
    | **Typ** | SQL Database | Użyj domyślnego połączenia bazy danych SQL. |   

3. Kliknij pozycję **Zapisz**.

Teraz można dodać kod funkcji języka C#, który nawiązuje połączenie z bazą danych SQL.

## <a name="update-your-function-code"></a>Zaktualizuj kod — funkcja

1. W funkcji aplikacji wybierz funkcję wyzwalany przez czasomierz.
 
3. Dodaj następujące odwołania do zestawów w górnej części istniejącego kodu funkcji:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. Dodaj następujące `using` instrukcje funkcji:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Zastąp istniejące **Uruchom** funkcja następującym kodem:
    ```cs
    public static async Task Run(TimerInfo myTimer, TraceWriter log)
    {
        var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " + 
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    To przykładowe polecenie aktualizuje **stan** kolumna jest oparta na daty. Należy ją zaktualizować 32 wierszy danych.

5. Kliknij przycisk **zapisać**, obejrzyj **dzienniki** windows Następna funkcja wykonywania, a następnie zanotuj liczbę wierszy zaktualizowane w **SalesOrderHeader** tabeli.

    ![Wyświetlać dzienniki funkcji.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Następne kroki

Następnie Dowiedz się, jak za pomocą funkcji aplikacji logiki do integracji z innymi usługami.

> [!div class="nextstepaction"] 
> [Tworzy funkcję, która integruje się z usługą Logic Apps](functions-twitter-email.md)

Aby uzyskać więcej informacji na temat funkcji zobacz następujące tematy:

* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)  
  Dokumentacja dla programistów dotycząca kodowania funkcji oraz definiowania wyzwalaczy i powiązań.
* [Testowanie usługi Azure Functions](functions-test-a-function.md)  
  Opis różnych narzędzi i technik testowania funkcji.  
