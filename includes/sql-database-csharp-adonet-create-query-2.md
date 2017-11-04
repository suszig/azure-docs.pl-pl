
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a>Przykład programu C#

Następne sekcje w tym artykule istnieje program C#, który używa ADO.NET do wysyłania instrukcji języka Transact-SQL do bazy danych SQL. C# program wykonuje następujące czynności:

1. [Łączy się z naszym bazy danych SQL za pomocą ADO.NET](#cs_1_connect).
2. [Tworzy tabele](#cs_2_createtables).
3. [Wypełnia tabel z danymi, wysyłając instrukcji INSERT T-SQL](#cs_3_insert).
4. [Aktualizuje dane przy użyciu sprzężenia](#cs_4_updatejoin).
5. [Usuwa dane przy użyciu sprzężenia](#cs_5_deletejoin).
6. [Wybiera wiersze danych przy użyciu sprzężenia](#cs_6_selectrows).
7. Zamyka połączenie (który porzuca wszystkie tabele tymczasowe z bazy danych tempdb).

Zawiera program C#:

- Kod C# do połączenia z bazą danych.
- Metody, które zwracają kod źródłowy T-SQL.
- Dwie metody, których przesłać T-SQL w bazie danych.

#### <a name="to-compile-and-run"></a>Aby skompilować i uruchomić

Ten program C# jest logicznie .cs plików. Jednak w tym miejscu program fizycznie jest podzielony na wiele bloków kodu, każdy blok ułatwiające Zobacz i zrozumieć. Aby skompilować i uruchomić ten program, wykonaj następujące czynności:

1. Tworzenie projektu C# w programie Visual Studio.
    - Typ projektu powinien być *konsoli* aplikacji z przypominać następująca hierarchia: **szablony** > **Visual C#** >  **System Windows Desktop klasycznego** > **konsoli aplikacji (.NET Framework)**.
3. W pliku **Program.cs**, Wymaż małych początkowe wiersze kodu.
3. W pliku Program.cs skopiuj i Wklej każdego z następujących bloków, w takiej samej kolejności, które są przedstawione w tym miejscu.
4. W pliku Program.cs edytować następujące wartości w **Main** metody:

   - **CB. Źródło danych**
   - **dysk CD. Nazwa użytkownika**
   - **CB. Hasło**
   - **InitialCatalog**

5. Sprawdź, czy zestaw **System.Data.dll** odwołuje się do. Aby sprawdzić, rozwiń węzeł **odwołania** w węźle **Eksploratora rozwiązań** okienka.
6. Aby utworzyć program w programie Visual Studio, kliknij przycisk **kompilacji** menu.
7. Aby uruchomić program z programu Visual Studio, kliknij przycisk **Start** przycisku. Dane wyjściowe raportu są wyświetlane w oknie cmd.exe.

> [!NOTE]
> Istnieje możliwość edycji T-SQL do dodania na początku  **#**  do nazwy tabeli, co powoduje ich jako tymczasowych tabel w **tempdb**. Może to być przydatne dla celów demonstracyjnych, gdy baza danych nie testu jest niedostępna. Tabele tymczasowe są usuwane automatycznie, gdy połączenie zostanie zamknięte. Wszystkie odwołania kluczy obcych nie są wymuszane dla tabel tymczasowych.
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a>Blok C# 1: Połącz przy użyciu pakietu ADO.NET

- [Dalej](#cs_2_createtables)


```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll 
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
   class Program
   {
      static void Main(string[] args)
      {
         try
         {
            var cb = new SqlConnectionStringBuilder();
            cb.DataSource = "your_server.database.windows.net";
            cb.UserID = "your_user";
            cb.Password = "your_password";
            cb.InitialCatalog = "your_database";

            using (var connection = new SqlConnection(cb.ConnectionString))
            {
               connection.Open();

               Submit_Tsql_NonQuery(connection, "2 - Create-Tables",
                  Build_2_Tsql_CreateTables());

               Submit_Tsql_NonQuery(connection, "3 - Inserts",
                  Build_3_Tsql_Inserts());

               Submit_Tsql_NonQuery(connection, "4 - Update-Join",
                  Build_4_Tsql_UpdateJoin(),
                  "@csharpParmDepartmentName", "Accounting");

               Submit_Tsql_NonQuery(connection, "5 - Delete-Join",
                  Build_5_Tsql_DeleteJoin(),
                  "@csharpParmDepartmentName", "Legal");

               Submit_6_Tsql_SelectEmployees(connection);
            }
         }
         catch (SqlException e)
         {
            Console.WriteLine(e.ToString());
         }
         Console.WriteLine("View the report output here, then press any key to end the program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
### <a name="c-block-2-t-sql-to-create-tables"></a>C# blok 2: T-SQL do tworzenia tabel

- [Poprzednie](#cs_1_connect) &nbsp;  /  &nbsp; [dalej](#cs_3_insert)

```csharp
      static string Build_2_Tsql_CreateTables()
      {
         return @"
DROP TABLE IF EXISTS tabEmployee;
DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.


CREATE TABLE tabDepartment
(
   DepartmentCode  nchar(4)          not null
      PRIMARY KEY,
   DepartmentName  nvarchar(128)     not null
);

CREATE TABLE tabEmployee
(
   EmployeeGuid    uniqueIdentifier  not null  default NewId()
      PRIMARY KEY,
   EmployeeName    nvarchar(128)     not null,
   EmployeeLevel   int               not null,
   DepartmentCode  nchar(4)              null
      REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
);
";
      }
```

#### <a name="entity-relationship-diagram-erd"></a>Diagram relacji jednostki (dysku Naprawczego)

Obejmują powyższych instrukcji CREATE TABLE **odwołania** — słowo kluczowe, aby utworzyć *klucz obcy* (klucz OBCY) relację między dwiema tabelami.  Jeśli używasz bazy danych tempdb komentarz `--REFERENCES` — słowo kluczowe przy użyciu pary wiodące kreski.

Następnie jest dysku Naprawczego, który przedstawia relację między dwiema tabelami. Wartości w #tabEmployee.DepartmentCode *podrzędnych* kolumny są ograniczone do wartości znajdujących się w #tabDepartment.Department *nadrzędnej* kolumny.

![Klucz obcy przedstawiający dysku Naprawczego](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-to-insert-data"></a>Blok C# 3: T-SQL do wstawiania danych

- [Poprzednie](#cs_2_createtables) &nbsp;  /  &nbsp; [dalej](#cs_4_updatejoin)


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- The company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- The company has these employees, each in one department.
INSERT INTO tabEmployee
   (EmployeeName, EmployeeLevel, DepartmentCode)
      VALUES
   ('Alison'  , 19, 'acct'),
   ('Barbara' , 17, 'hres'),
   ('Carol'   , 21, 'acct'),
   ('Deborah' , 24, 'legl'),
   ('Elle'    , 15, null);
";
      }
```


<a name="cs_4_updatejoin"/>
### <a name="c-block-4-t-sql-to-update-join"></a>C# bloku 4: T-SQL do przyłączania do aktualizacji

- [Poprzednie](#cs_3_insert) &nbsp;  /  &nbsp; [dalej](#cs_5_deletejoin)


```csharp
      static string Build_4_Tsql_UpdateJoin()
      {
         return @"
DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';


-- Promote everyone in one department (see @parm...).
UPDATE empl
   SET
      empl.EmployeeLevel += 1
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName1;
";
      }
```


<a name="cs_5_deletejoin"/>
### <a name="c-block-5-t-sql-to-delete-join"></a>Blok C# 5: T-SQL do przyłączania do usunięcia

- [Poprzednie](#cs_4_updatejoin) &nbsp;  /  &nbsp; [dalej](#cs_6_selectrows)


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size the Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband the Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
### <a name="c-block-6-t-sql-to-select-rows"></a>Blok C# 6: T-SQL, aby wybrać wiersze

- [Poprzednie](#cs_5_deletejoin) &nbsp;  /  &nbsp; [dalej](#cs_6b_datareader)


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all the final Employees.
SELECT
      empl.EmployeeGuid,
      empl.EmployeeName,
      empl.EmployeeLevel,
      empl.DepartmentCode,
      dept.DepartmentName
   FROM
      tabEmployee   as empl
      LEFT OUTER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   ORDER BY
      EmployeeName;
";
      }
```


<a name="cs_6b_datareader"/>
### <a name="c-block-6b-executereader"></a>C# bloku 6b: ExecuteReader

- [Poprzednie](#cs_6_selectrows) &nbsp;  /  &nbsp; [dalej](#cs_7_executenonquery)

Ta metoda jest przeznaczona do Uruchom instrukcję SELECT T-SQL jest konstruowany przez **Build_6_Tsql_SelectEmployees** metody.


```csharp
      static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("Now, SelectEmployees (6)...");

         string tsql = Build_6_Tsql_SelectEmployees();

         using (var command = new SqlCommand(tsql, connection))
         {
            using (SqlDataReader reader = command.ExecuteReader())
            {
               while (reader.Read())
               {
                  Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                     reader.GetGuid(0),
                     reader.GetString(1),
                     reader.GetInt32(2),
                     (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                     (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
               }
            }
         }
      }
```


<a name="cs_7_executenonquery"/>
### <a name="c-block-7-executenonquery"></a>C# bloku 7: ExecuteNonQuery

- [Poprzednie](#cs_6b_datareader) &nbsp;  /  &nbsp; [dalej](#cs_8_output)

Ta metoda jest wywoływana w operacjach modyfikujące zawartości danych tabel bez zwracanie wszystkich wierszy danych.


```csharp
      static void Submit_Tsql_NonQuery(
         SqlConnection connection,
         string tsqlPurpose,
         string tsqlSourceCode,
         string parameterName = null,
         string parameterValue = null
         )
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("T-SQL to {0}...", tsqlPurpose);

         using (var command = new SqlCommand(tsqlSourceCode, connection))
         {
            if (parameterName != null)
            {
               command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                  parameterName,
                  parameterValue);
            }
            int rowsAffected = command.ExecuteNonQuery();
            Console.WriteLine(rowsAffected + " = rows affected.");
         }
      }
   } // EndOfClass
}
```


<a name="cs_8_output"/>
### <a name="c-block-8-actual-test-output-to-the-console"></a>C# bloku 8: wyniki testu rzeczywiste do konsoli

- [Poprzednie](#cs_7_executenonquery)

Ta sekcja zawiera dane wyjściowe programu wysyłane do konsoli. Tylko wartości identyfikatora guid różnią się pomiędzy uruchomieniami testów.


```text
[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>> csharp_db_test.exe

=================================
Now, CreateTables (10)...

=================================
Now, Inserts (20)...

=================================
Now, UpdateJoin (30)...
2 rows affected, by UpdateJoin.

=================================
Now, DeleteJoin (40)...

=================================
Now, SelectEmployees (50)...
0199be49-a2ed-4e35-94b7-e936acf1cd75 , Alison , 20 , acct , Accounting
f0d3d147-64cf-4420-b9f9-76e6e0a32567 , Barbara , 17 , hres , Human Resources
cf4caede-e237-42d2-b61d-72114c7e3afa , Carol , 22 , acct , Accounting
cdde7727-bcfd-4f72-a665-87199c415f8b , Elle , 15 , NULL , NULL

[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>>
```
