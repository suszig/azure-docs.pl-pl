## <a name="repeatability-during-copy"></a>Powtarzalność podczas kopiowania
Podczas kopiowania danych z i do magazynów relacyjne, musisz należy pamiętać, aby uniknąć niezamierzone wyniki powtarzalności. 

Wycinek można uruchomić ponownie automatycznie w fabryce danych Azure zgodnie z harmonogramem zasady ponawiania określone. Firma Microsoft zaleca ustawienie zasady ponawiania ochrona przed awariami przejściowej. Dlatego powtarzalności jest istotnym elementem do obsługi podczas przenoszenia danych. 

**Jako źródło:**

> [!NOTE]
> Poniższe przykłady są dla bazy danych SQL Azure, ale są stosowane do dowolnego magazynem danych, który obsługuje prostokątne zestawów danych. Może być konieczne dostosowanie **typu** źródła i **zapytania** właściwości (na przykład: zapytanie, zamiast sqlReaderQuery) dla danych przechowywania.   
> 
> 

Zazwyczaj podczas odczytu z relacyjnych magazynów, czy chcesz odczytać tylko danych dotyczących tego wycinka. Jest sposób, aby to zrobić za pomocą zmiennych WindowStart i WindowEnd, które muszą być dostępne w fabryce danych Azure. Przeczytaj informacje o zmiennych i funkcji w fabryce danych Azure w tym miejscu w [planowania i wykonywania](../articles/data-factory/v1/data-factory-scheduling-and-execution.md) artykułu. Przykład: 

```json
"source": {
"type": "SqlSource",
"sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
To zapytanie odczytuje dane z MyTable, która znajduje się w zakresie czasu trwania wycinka. Uruchom ponownie tego wycinka będzie zawsze upewnij się, to zachowanie. 

W pozostałych przypadkach warto przeczytać całą tabelę (Załóżmy, że dla jednorazowo przenieść tylko) i może zdefiniować sqlReaderQuery w następujący sposób:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```
