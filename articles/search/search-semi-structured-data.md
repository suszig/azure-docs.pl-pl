---
title: "Wyszukiwanie częściowo ustrukturyzowanych danych w magazynie w chmurze Azure"
description: "Wyszukiwanie danych z częściową strukturą obiektów blob przy użyciu usługi Azure Search."
author: roygara
manager: timlt
ms.service: search
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: a80ae99c2ada00885019ee93e4ef36821340d3a5
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/13/2018
---
# <a name="part-2-search-semi-structured-data-in-cloud-storage"></a>Część 2: Wyszukiwanie częściowo ustrukturyzowanych danych w magazynie w chmurze

W dwóch części samouczka serii Dowiedz się jak wyszukiwanie częściową strukturą i bez struktury danych za pomocą usługi Azure search. [Część 1](../storage/blobs/storage-unstructured-search.md) udał można za pomocą funkcji wyszukiwania danych bez struktury, ale również uwzględnione ważne wymagania wstępne dotyczące tego samouczka, takich jak tworzenie konta magazynu. 

W części 2 otrzymuje fokus częściowo ustrukturyzowanych danych, takich jak JSON przechowywane w obiektach blob Azure. Częściowo ustrukturyzowanych danych zawiera tagi lub oznaczenia, które dzielą zawartości w danych. Dzieli różnicę między danych bez struktury, która musi zostać pomyślnie zindeksowane wholistically i danych strukturalnych formalnie, zgodną z modelem danych, takie jak schemat relacyjnej bazy danych, które mogą być przeszukiwane na podstawie na pola.

W części 2, Dowiedz się, jak:

> [!div class="checklist"]
> * Konfigurowanie źródła danych usługi Azure Search dla kontenera obiektów blob platformy Azure
> * Tworzenia i wypełniania indeksu usługi Azure Search i indeksator przeszukiwania kontenera i Wyodrębnij zawartość wyszukiwanie
> * W indeksie utworzony

> [!NOTE]
> W tym samouczku korzysta z obsługi tablicy JSON, który jest obecnie w wersji zapoznawczej w usłudze Azure Search. Nie jest dostępne w portalu. Z tego powodu firma Microsoft korzysta z interfejsu API REST, który zapewnia tę funkcję, a pozostałe narzędzia klienta do wywołania interfejsu API w wersji zapoznawczej.

## <a name="prerequisites"></a>Wymagania wstępne

* Zakończenie [poprzedniego samouczek](../storage/blobs/storage-unstructured-search.md) świadczenie usług magazynu konto i wyszukiwania utworzonej w poprzednim samouczku.

* Instalacja klienta REST i zrozumienia sposobu tworzenia żądania HTTP. Do celów tego samouczka, użyto [Postman](https://www.getpostman.com/). Możesz także użyć różnych klienta REST, jeśli już znasz jedna z nich.

## <a name="set-up-postman"></a>Konfigurowanie Postman

Uruchom Postman i skonfigurować żądania HTTP. Jeśli nie znasz z tego narzędzia, zobacz [Eksplorowanie usługi Azure Search interfejsów API REST przy użyciu narzędzia Fiddler lub Postman](search-fiddler.md) Aby uzyskać więcej informacji.

Metoda żądania dla każdego wywołania w tym samouczku jest "POST". Są klucze nagłówek "Content-type" i "api-key". Wartości nagłówka kluczy są "application/json" i klucz"Administrator" (klucz administratora to symbol zastępczy wyszukiwania primary key) odpowiednio. Treść jest, gdzie umieścić rzeczywistej zawartości wywołania. W zależności od klienta, którego używasz może być różne ich warianty na sposób tworzenia kwerendy, ale są podstawy.

  ![Częściowo ustrukturyzowanych wyszukiwania](media/search-semi-structured-data/postmanoverview.png)

Wywołania REST omówione w tym samouczku wymagany jest klucz interfejsu api wyszukiwania. Klucz interfejsu api w obszarze można znaleźć **klucze** wewnątrz usługi wyszukiwania. Ten klucz interfejsu api musi być w nagłówku każdego wywołania interfejsu API (Zastąp "klucz administratora" na poprzednim zrzucie ekranu z nim) w tym samouczku kieruje można utworzyć. Klucz należy zachować, ponieważ należy dla każdego wywołania.

  ![Częściowo ustrukturyzowanych wyszukiwania](media/search-semi-structured-data/keys.png)

## <a name="download-the-sample-data"></a>Pobierz przykładowe dane

Przykładowy zestaw danych został przygotowany dla Ciebie. **Pobierz [klinicznych json.zip prób](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)**  i Rozpakuj go do własnego folderu.

Przykład zawartych w próbce są pliki w formacie JSON, które pierwotnie tekst pliki uzyskane z [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). Firma Microsoft zostaną przekonwertowane na format JSON dla Twojej wygody.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do witryny [Azure Portal](http://portal.azure.com).

## <a name="upload-the-sample-data"></a>Przekaż przykładowe dane

W portalu Azure, przejdź wstecz do na koncie magazynu utworzonym w [samouczek poprzedniej](../storage/blobs/storage-unstructured-search.md). Następnie otwórz **danych** kontenera, a następnie kliknij przycisk **przekazać**.

Kliknij przycisk **zaawansowane**, wprowadź "json klinicznych prób", a następnie Przekaż wszystkie pobrane pliki w formacie JSON.

  ![Częściowo ustrukturyzowanych wyszukiwania](media/search-semi-structured-data/clinicalupload.png)

Po ukończeniu przekazywania plików powinny być wyświetlane w podfolderze własnych wewnątrz kontenera danych.

## <a name="connect-your-search-service-to-your-container"></a>Nawiązać połączenie z kontenera usługi wyszukiwania

Użyto Postman dokonanie trzy wywołania interfejsu API usługi wyszukiwania w celu tworzenia źródła danych, indeksu i indeksatora. Źródło danych zawiera wskaźnik do konta magazynu i dane JSON. Usługi wyszukiwania nawiązuje połączenie podczas ładowania danych.

Ciąg zapytania musi zawierać **interfejsu api-version = 2016-09-01-Preview** i każde wywołanie powinien zwrócić **201 utworzono**. Ogólnie dostępna wersja interfejsu api nie ma jeszcze możliwość obsługi formatu json jako jsonArray, obecnie jest tylko interfejs api — wersja.

Wykonaj następujące trzy wywołania interfejsu API z klienta REST.

### <a name="create-a-datasource"></a>Tworzenie źródła danych

Źródło danych określa, jakie dane do indeksu.

Punkt końcowy tego wywołania jest `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Zastąp `[service name]` o nazwie usługi wyszukiwania.

Dla tego wywołania jest potrzebna nazwa konta magazynu i klucz konta magazynu. Klucz konta magazynu można znaleźć w portalu Azure w ramach konta magazynu **klucze dostępu**. Wyświetlana jest lokalizacja na poniższej ilustracji:

  ![Częściowo ustrukturyzowanych wyszukiwania](media/search-semi-structured-data/storagekeys.png)

Upewnij się zastąpić `[storage account name]` i `[storage account key]` w treści wywołania przed wykonaniem połączenia.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

Odpowiedź powinna wyglądać:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "data",
        "query": "clinical-trials-json"
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

### <a name="create-an-index"></a>Tworzenie indeksu
    
Drugie wywołanie interfejsu API tworzy indeks. Indeks określa wszystkich parametrów i ich atrybutów.

Adres URL dla tego wywołania jest `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. Zastąp `[service name]` o nazwie usługi wyszukiwania.

Najpierw Zastąp adres URL. Następnie skopiuj i wklej następujący kod do ciała i uruchomić zapytanie.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

Odpowiedź powinna wyglądać:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

### <a name="create-an-indexer"></a>Utwórz indeksator

Indeksator nawiązuje połączenie ze źródłem danych do indeksu wyszukiwania docelowej i opcjonalnie zapewnia harmonogramu do automatyzowania odświeżania danych.

Adres URL dla tego wywołania jest `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Zastąp `[service name]` o nazwie usługi wyszukiwania.

Najpierw Zastąp adres URL. Następnie skopiuj i wklej następujący kod do ciała i uruchomić zapytanie.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Odpowiedź powinna wyglądać:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Wyszukaj pliki w formacie JSON

Teraz, gdy usługi wyszukiwania został połączony z kontenerem danych, możesz rozpocząć wyszukiwanie plików.

Otwieranie portalu Azure i przejdź z powrotem do usługi wyszukiwania. Podobnie jak w poprzednich instrukcji.

  ![Bez struktury wyszukiwania](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Wyszukiwanie metadanych zdefiniowanych przez użytkownika

Jak wcześniej, dane można wyświetlić na kilka sposobów: wyszukiwanie pełnotekstowe, właściwości systemu lub metadanych zdefiniowanych przez użytkownika. Właściwości systemu jak metadanych zdefiniowanych przez użytkownika może tylko przeszukiwane z `$select` parametru, jeśli zostały oznaczone jako **pobieranie** podczas tworzenia indeksu docelowego. Nie można zmienić parametrów w indeksie, po ich utworzeniu. Jednak można dodać dodatkowe parametry.

Na przykład zapytania podstawowego `$select=Gender,metadata_storage_size`, co ogranicza powrotu do tych dwóch parametrów.

  ![Częściowo ustrukturyzowanych wyszukiwania](media/search-semi-structured-data/lastquery.png)

Oto przykład bardziej złożonego zapytania `$filter=MinimumAge ge 30 and MaximumAge lt 75`, która zwraca tylko wyniki, gdzie parametr MinimumAge jest większa niż lub równa 30 i MaximumAge jest mniejsza niż 75.

  ![Częściowo ustrukturyzowanych wyszukiwania](media/search-semi-structured-data/metadatashort.png)

Jeśli chcesz wypróbować, a następnie spróbuj kilka zapytań więcej samodzielnie, możesz w tym celu. Wiedzieć, że można używać operatorów logicznych (i, lub nie) i operatory porównania (eq, ne, gt, lt, ge, le, and). Porównywanie ciągów jest rozróżniana wielkość liter.

`$filter` Parametr działa tylko z metadanych, które zostały oznaczone jako filtrowanie podczas tworzenia indeksu.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku poznanie wyszukiwanie częściowo ustrukturyzowanych danych przy użyciu wyszukiwanie Azure, np.:

> [!div class="checklist"]
> * Tworzenie usługi wyszukiwanie Azure przy użyciu interfejsu API REST
> * Umożliwia wyszukiwanie z kontenera usługi wyszukiwanie Azure

Wykonaj to łącze, aby dowiedzieć się więcej na temat wyszukiwania.

> [!div class="nextstepaction"]
> [Indeksowanie dokumentów w magazynie obiektów Blob platformy Azure](search-howto-indexing-azure-blob-storage.md)