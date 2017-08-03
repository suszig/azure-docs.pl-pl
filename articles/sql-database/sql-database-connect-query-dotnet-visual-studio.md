---
title: "Korzystanie z programów Visual Studio i .NET do wykonywania zapytań w bazie danych Azure SQL | Microsoft Docs"
description: "W tym temacie przedstawiono sposób wykorzystania programu Visual Studio do utworzenia programu, który nawiązuje połączenie z bazą danych SQL Azure i wykonuje zapytania za pomocą instrukcji języka Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 105dab17823a7e7f6957a604833f4ecad35c14bd
ms.contentlocale: pl-pl
ms.lasthandoff: 07/28/2017

---
# <a name="use-net-c-with-visual-studio-to-connect-and-query-an-azure-sql-database"></a>Korzystanie z programu .NET (C#) do nawiązywania połączenia i wykonywania zapytań w bazie danych Azure SQL

W tym przewodniku Szybki start pokazano, jak używać [platformy .NET](https://www.microsoft.com/net/) w celu utworzenia programu w języku C# służącego do nawiązywania połączenia z bazą danych Azure SQL, a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten samouczek Szybki start, upewnij się, że dysponujesz następującymi elementami:

- Baza danych Azure SQL. Ten przewodnik Szybki start używa zasobów utworzonych w jednym z poniższych przewodników Szybki start: 

   - [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
   - [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
   - [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

- [Reguła zapory poziomu serwera](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) dla publicznego adresu IP komputera, który będzie używany w tym samouczku Szybki start.
- Zainstalowany program [Visual Studio Community 2017, Visual Studio Professional 2017 lub Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

## <a name="sql-server-connection-information"></a>Informacje o połączeniu z serwerem SQL

Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z bazą danych Azure SQL. W następnych procedurach będą potrzebne w pełni kwalifikowana nazwa serwera, nazwa bazy danych i informacje logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. Na stronie **Przegląd** bazy danych zweryfikuj w pełni kwalifikowaną nazwę serwera, jak pokazano na poniższej ilustracji. Możesz umieścić kursor na nazwie serwera w celu wywołania opcji **Kliknij, aby skopiować**. 

   ![nazwa-serwera](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Jeśli nie pamiętasz informacji logowania dla serwera Azure SQL Database, przejdź do strony serwera SQL Database, aby wyświetlić nazwę administratora serwera. W razie potrzeby zresetuj hasło.

5. Kliknij pozycję **Pokaż parametry połączenia bazy danych**.

6. Sprawdź pełne parametry połączenia sterownika **ADO.NET**.

    ![Parametry połączenia sterownika ADO.NET](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> W przypadku publicznego adresu IP komputera, na którym jest wykonywany ten samouczek, niezbędne jest posiadanie reguły zapory. Jeśli pracujesz na innym komputerze lub masz inny publiczny adres IP, utwórz [regułę zapory poziomu serwera przy użyciu portalu Azure](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 
>
  
## <a name="create-a-new-visual-studio-project"></a>Tworzenie nowego projektu programu Visual Studio

1. W programie Visual Studio wybierz pozycje **Plik**, **Nowy**, **Projekt**. 
2. W oknie dialogowym **Nowy projekt** rozwiń pozycję **Visual C#**.
3. Wybierz opcję **Aplikacja konsoli**, a następnie jako nazwę projektu wpisz *sqltest*.
4. Kliknij przycisk **OK**, aby utworzyć nowy projekt w programie Visual Studio i otworzyć go.
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy pozycję **sqltest**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**. 
5. W obszarze **Przeglądaj** wyszukaj pozycję ```System.Data.SqlClient``` i po znalezieniu wybierz ją.
6. Na stronie **System.Data.SqlClient** kliknij przycisk **Instaluj**.
7. Po zakończeniu instalacji przejrzyj zmiany, a następnie kliknij przycisk **OK**, aby zamknąć okno **Podgląd**. 
8. W przypadku wyświetlenia okna **Akceptacja licencji** kliknij przycisk **Akceptuję**.

## <a name="insert-code-to-query-sql-database"></a>Wstawianie kodu zapytania bazy danych SQL
1. Przełącz do obszaru **Program.cs** (lub otwórz go w razie potrzeby)

2. Zastąp zawartość obszaru **Program.cs** następującym kodem i dodaj odpowiednie wartości dla serwera, bazy danych, użytkownika i hasła.

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
{
    class Program
    {
        static void Main(string[] args)
        {
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
            Console.ReadLine();
        }
    }
}
```

## <a name="run-the-code"></a>Uruchamianie kodu

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.
2. Sprawdź, czy zostało zwróconych 20 pierwszych wierszy, a następnie zamknij okno aplikacji.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [uzyskać połączenie i wykonywać zapytania bazy danych Azure SQL przy użyciu platformy .NET Core](sql-database-connect-query-dotnet-core.md) w systemach operacyjnych Windows/Linux/macOS.  
- Dowiedz się więcej o [rozpoczynaniu pracy z platformą .NET Core w systemie Windows/Linux/macOS przy użyciu wiersza polecenia](/dotnet/core/tutorials/using-with-xplat-cli).
- Dowiedz się, jak [zaprojektować pierwszą bazę danych Azure SQL przy użyciu narzędzia SSMS](sql-database-design-first-database.md) lub [zaprojektować pierwszą bazę danych Azure SQL przy użyciu platformy .NET](sql-database-design-first-database-csharp.md).
- Aby uzyskać więcej informacji na temat platformy .NET, zobacz [.NET documentation](https://docs.microsoft.com/dotnet/) (Dokumentacja platformy .NET).

