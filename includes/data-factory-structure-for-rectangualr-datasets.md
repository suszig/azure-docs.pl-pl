## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Określanie struktury definicji dla prostokątnego zestawów danych
Sekcja struktury w zestawach danych JSON jest **opcjonalne** sekcji prostokątne tabel (wiersze i kolumny) i zawiera zestaw kolumn dla tabeli. Struktura sekcji użyje dla obu udostępnienie informacji o typie dla konwersje typów lub wykonując mapowania kolumn. W poniższych sekcjach opisano te funkcje szczegółowo. 

Każda kolumna zawiera następujące właściwości:

| Właściwość | Opis | Wymagane |
| --- | --- | --- |
| name |Nazwa kolumny. |Tak |
| type |Typ danych kolumny. Zobacz szczegółowe typu konwersje poniższej sekcji dotyczące kiedy należy określić informacje o typie |Nie |
| Kultury |.NET na podstawie kultury, który będzie używany podczas typu określono i jest typ architektury .NET, Datetime i Datetimeoffset. Domyślna to "en-us". |Nie |
| Format |Ciąg formatu do użycia, gdy jest określony typ i .NET typu Datetime i Datetimeoffset. |Nie |

Poniższy przykład przedstawia sekcji struktury JSON dla tabeli, która ma trzy kolumny userid, nazwy i lastlogindate.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Użyj następujących wytycznych umieszczania "struktury" informacji i elementów do uwzględnienia w **struktury** sekcji.

* **Dla źródeł danych strukturalnych** że magazynu schematu i typu danych oraz dane (źródeł, takich jak SQL Server, Oracle, tabeli platformy Azure itp.), należy określić w sekcji "structure" tylko wtedy, gdy chcesz wykonać mapowanie kolumn z określonego źródła kolumny do określonych kolumn w ich nazwy i odbiorczy nie są takie same (Zobacz szczegóły w poniższej sekcji mapowanie kolumn). 
  
    Jak wspomniano powyżej, informacje o typie jest opcjonalna w sekcji "structure". Strukturalne źródeł informacji o typie jest już dostępne w ramach definicji zestawu danych w magazynie danych, dlatego nie należy używać informacji o typie po dołączeniu w sekcji "structure".
* **Do schematu w źródłach danych odczytu (w szczególności obiektów blob platformy Azure)** można przechowywać dane bez przechowywania żadnych informacji schematu lub typ z danymi. Dla tych typów źródeł danych, należy uwzględnić "structure" w następujących przypadkach 2:
  * Chcesz zrobić mapowania kolumn.
  * Zestaw danych jest źródłem w działaniu kopiowania, możesz podać informacje o typie w "structure", a fabryki danych użyje tych informacji typu do konwersji na typy natywne dla obiekt sink. Zobacz [przenoszenie danych do i z obiektu Blob Azure](../articles/data-factory/v1/data-factory-azure-blob-connector.md) artykułu, aby uzyskać więcej informacji.

### <a name="supported-net-based-types"></a>Obsługiwane. Typy sieci
Fabryki danych obsługuje następujące ze specyfikacją CLS zgodne .NET na podstawie typu wartości udostępnienie informacji o typie w "structure" dla schematu na źródeł danych odczytu, takich jak obiektów blob platformy Azure.

* Int16
* Int32 
* Int64
* Pojedynczy
* O podwójnej precyzji
* Decimal
* Byte]
* wartość logiczna
* Ciąg 
* Identyfikator GUID
* Data i godzina
* Datetimeoffset
* Zakres czasu 

Dla typu Datetime i Datetimeoffset również opcjonalnie można określić ciąg "Kultura" & "format", aby ułatwić analizowanie niestandardowego ciągu daty/godziny. Zobacz przykład poniżej konwersji typu.

