## <a name="fileshare-dataset-type-properties"></a>Właściwości typu zestawu danych w udziale plików
Aby uzyskać pełną listę sekcje & właściwości dostępne do definiowania zestawów danych, zobacz [Tworzenie zbiorów danych](../articles/data-factory/v1/data-factory-create-datasets.md) artykułu. Sekcje zawierają informacje, takie jak struktury, dostępności i zasad zestawu danych JSON są podobne dla wszystkich typów w zestawie danych.

**TypeProperties** sekcja jest różne dla każdego typu zestawu danych. Zawiera informacje specyficzne dla typu zestawu danych. TypeProperties sekcja dla zestawu danych typu **FileShare** zestawu danych ma następujące właściwości:

| Właściwość | Opis | Wymagane |
| --- | --- | --- |
| folderPath |Sub ścieżkę do folderu. Użyj znaku ucieczki "\" dla znaków specjalnych w ciągu. Zobacz [próbki połączone definicje usługi i zestawu danych](#sample-linked-service-and-dataset-definitions) przykłady.<br/><br/>Możesz łączyć tej właściwości z **partitionBy** do folderu ścieżki oparte na wycinku rozpoczęcia/zakończenia daty i godziny. |Tak |
| fileName |Określ nazwę pliku w **folderPath** aby tabela do odwoływania się do określonego pliku w folderze. Jeśli nie określono żadnej wartości dla tej właściwości, tabela wskazuje wszystkie pliki w folderze.<br/><br/>Jeśli nie określono nazwy pliku dla wyjściowego zestawu danych, nazwę wygenerowanego pliku będzie poniżej tego formatu: <br/><br/>Dane. <Guid>.txt (przykład: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nie |
| obiektu fileFilter |Określ filtr służący do wybierania podzbioru pliki w ścieżce folderu, a nie wszystkie pliki.<br/><br/>Dozwolone wartości to: `*` (wielu znaków) i `?` (pojedynczy znak).<br/><br/>Przykład 1:`"fileFilter": "*.log"`<br/>Przykład 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> obiektu fileFilter jest odpowiednie dla wejściowego zestawu danych z udziału plików. Ta właściwość nie jest obsługiwana z systemu plików HDFS. |Nie |
| partitionedBy |partitionedBy może służyć do określenia dynamiczne folderPath, nazwę pliku dla czasu serii danych. Na przykład folderPath sparametryzowana dla każdej godziny danych. |Nie |
| Format | Obsługiwane są następujące typy format: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ustaw **typu** właściwości w formacie do jednej z tych wartości. Aby uzyskać więcej informacji, zobacz [formacie tekstowym](#specifying-textformat), [formatu Json](#specifying-jsonformat), [Avro Format](#specifying-avroformat), [Orc Format](#specifying-orcformat), i [Parquet Format](#specifying-parquetformat) sekcje. <br><br> Jeśli chcesz **skopiuj pliki jako — jest** między opartych na plikach magazynów (kopia binarnego), Pomiń sekcji format w obu definicji zestawu danych wejściowych i wyjściowych. |Nie |
| Kompresja | Określ typ i poziom kompresji danych. Obsługiwane typy to: **GZip**, **Deflate**, **BZip2**, i **ZipDeflate**; i są obsługiwane poziomy: **optymalna** i **najszybciej**. Aby uzyskać więcej informacji, zobacz [określenie kompresji](#specifying-compression) sekcji. |Nie |
| useBinaryTransfer |Określ, czy używany tryb transferu binarnego. Wartość true dla trybie binarnym i wartość false ASCII. Wartość domyślna: wartość True. Ta właściwość jest używana tylko w przypadku typu skojarzonej połączonej usługi typu: SerwerFTP. |Nie |

> [!NOTE]
> Nie można jednocześnie używać nazwy pliku i obiektu fileFilter.
>
>

### <a name="using-partionedby-property"></a>Za pomocą właściwości partionedBy
Jak wspomniano w poprzedniej sekcji, można określić dynamiczne folderPath, nazwę pliku dla danych szeregów czasowych ze partitionedBy. Możesz to zrobić z makra fabryki danych i zmiennej systemowej SliceStart, SliceEnd sygnalizujące logicznej okresu wycinek podanych danych.

Informacje na temat zestawów danych serii czasu, planowanie i wycinków, zobacz [tworzenie zestawów danych](../articles/data-factory/v1/data-factory-create-datasets.md), [planowanie i wykonanie](../articles/data-factory/v1/data-factory-scheduling-and-execution.md), i [tworzenie potoków](../articles/data-factory/v1/data-factory-create-pipelines.md) artykułów.

#### <a name="sample-1"></a>Przykład 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
W tym przykładzie {wycinka} zostanie zastąpiony wartością zmiennej systemowej SliceStart fabryki danych w formacie (YYYYMMDDHH) określona. SliceStart odnosi się uruchomienie wycinka. Element folderPath jest różne dla każdego wycinka. Przykład: wikidatagateway/wikisampledataout/2014100103 lub wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Przykład 2:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
W tym przykładzie rok, miesiąc, dzień i czas SliceStart są wyodrębniane do oddzielnych zmiennych, które są używane przez właściwości folderPath i nazwę pliku.
