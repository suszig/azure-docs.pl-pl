## <a name="repeatability-during-copy"></a>Powtarzalność podczas kopiowania
Jeśli kopiowanie danych do usługi Azure SQL/programu SQL Server z innymi danymi przechowuje należy należy pamiętać, aby uniknąć niezamierzone wyniki powtarzalności. 

Podczas kopiowania danych do bazy danych serwera SQL/SQL Azure, działanie kopiowania zostanie domyślnie Dołącz zestaw danych do tabeli ujścia domyślnie. Na przykład podczas kopiowania danych z źródła pliku CSV (dane wartości rozdzielonych przecinkami) zawierającego dwa rekordy do bazy danych serwera SQL/SQL Azure, jest tabela wygląda następująco:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Załóżmy, że znaleziono błędy w pliku źródłowym i zaktualizować ilość przewodu w dół od 2 do 4 w pliku źródłowym. Jeśli uruchomisz ponownie wycinek danych dla tego okresu, znajdują się dwie nowe rekordy dołączane do bazy danych serwera SQL/SQL Azure. Poniżej założono, żaden z kolumn w tabeli nie ma ograniczenia klucza podstawowego.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Aby tego uniknąć, należy określić semantykę UPSERT przez wykorzystanie jednej z poniższych 2 mechanizmów podaną poniżej.

> [!NOTE]
> Wycinek może zostać ponownie uruchomione automatycznie w fabryce danych Azure zgodnie z harmonogramem zasady ponawiania określone.
> 
> 

### <a name="mechanism-1"></a>Mechanizm 1
Można wykorzystać **sqlWriterCleanupScript** właściwości, aby najpierw wykonać akcję czyszczenia po uruchomieniu wycinek. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Skrypt czyszczący będzie można wykonać pierwsze podczas kopiowania dla danego wycinka, co spowoduje usunięcie danych z tabeli SQL odpowiadający tego wycinka. Działanie zostanie następnie wstawianie danych do tabeli SQL. 

Jeśli wycinek jest teraz ponownie uruchomić, a następnie można tam znaleźć ilość jest aktualizowana jako wymaganą.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Załóżmy, że rekord płaskiej podkładka zostanie usunięte z oryginalnej csv. Ponowne uruchomienie wycinka dałby w efekcie następujące wyniki: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Żadne nowe musiało być przeprowadzane. Działanie kopiowania uruchomiono skrypt czyszczący do usuwania odpowiednich danych dla tego wycinka. A następnie go odczytać dane wejściowe z plików csv (który następnie zawiera tylko 1 rekord) i dodaje go do tabeli. 

### <a name="mechanism-2"></a>Mechanizm 2
> [!IMPORTANT]
> w tej chwili sliceIdentifierColumnName nie jest obsługiwana dla usługi Azure SQL Data Warehouse. 

Innym mechanizmem do osiągnięcia powtarzalności jest wprowadzenie dedykowanego kolumny (**sliceIdentifierColumnName**) w tabeli docelowej. W tej kolumnie będzie służyć przez fabryki danych Azure, aby upewnić się, że na serwerze źródłowym i docelowym zachować synchronizację. Ta metoda działa po elastyczność zmiana lub definiowania schematu SQL tabeli docelowej. 

Ta kolumna będzie używany przez fabryki danych Azure na potrzeby celów powtarzalność i w procesie fabryki danych Azure nie dokona żadnych zmian schematu do tabeli. Sposób użycia tej metody:

1. W docelowej tabeli SQL, należy zdefiniować kolumnę z danymi typu binarnego (32). Nie powinno być nie ograniczeń dla tej kolumny. Teraz nazwę tej kolumny jako "ColumnForADFuseOnly" w tym przykładzie.
2. Należy użyć go w przypadku działania kopiowania w następujący sposób:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Fabryka danych Azure są powielane tej kolumny, trzeba upewnić się, że na serwerze źródłowym i docelowym zachować synchronizację zgodnie z harmonogramem. Wartości tej kolumny powinien nie używane poza tym kontekście przez użytkownika. 

Podobnie do mechanizmu 1, działanie kopiowania zostanie automatycznie najpierw wyczyścić dane dla wycinka danego przeznaczenia tabeli SQL, a następnie uruchomić działanie kopiowania normalnie, aby wstawić dane ze źródła do miejsca docelowego dla tego wycinka. 

