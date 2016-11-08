---
title: Tworzenie indeksu usługi Azure Search przy użyciu zestawu .NET SDK | Microsoft Docs
description: Tworzenie indeksu za pomocą kodu przy użyciu zestawu .NET SDK usługi Azure Search.
services: search
documentationcenter: ''
author: brjohnstmsft
manager: ''
editor: ''
tags: azure-portal

ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: brjohnst

---
# Tworzenie indeksu usługi Azure Search przy użyciu zestawu .NET SDK
> [!div class="op_single_selector"]
> * [Omówienie](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Ten artykuł przeprowadzi Cię przez proces tworzenia [indeksu](https://msdn.microsoft.com/library/azure/dn798941.aspx) usługi Azure Search przy użyciu [zestawu .NET SDK usługi Azure Search](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Przed rozpoczęciem pracy z przewodnikiem oraz przed utworzeniem indeksu powinna zostać [utworzona usługa Azure Search](search-create-service-portal.md).

Należy zwrócić uwagę, że cały przykładowy kod przedstawiony w tym artykule został napisany w języku C#. Pełny kod źródłowy można znaleźć [w usłudze GitHub](http://aka.ms/search-dotnet-howto).

## I. Identyfikowanie klucza api-key administratora usługi Azure Search
Po aprowizowaniu usługi Azure Search wszystko jest już prawie gotowe, aby móc wysyłać żądania do punktu końcowego usługi za pomocą zestawu .NET SDK. Najpierw należy uzyskać klucz api-key administratora, który został wygenerowany dla aprowizowanej usługi wyszukiwania. Zestaw .NET SDK przesyła ten klucz przy każdorazowym wysłaniu żądania do usługi. Prawidłowy klucz ustanawia relację zaufania dla danego żądania między aplikacją wysyłającą żądanie i usługą, która je obsługuje.

1. Aby odnaleźć klucze api-key dla usługi, musisz zalogować się w witrynie [Azure Portal](https://portal.azure.com/)
2. Przejdź do bloku usługi Azure Search
3. Kliknij ikonę „Klucze”

Usługa będzie dysponować *kluczami administratora* i *kluczami zapytań*.

* Za pomocą podstawowego i pomocniczego *klucza administratora* przyznawane są pełne prawa do wszystkich operacji, łącznie z możliwością zarządzania usługą oraz tworzenia i usuwania indeksów, indeksatorów i źródeł danych. Dostępne są dwa klucze, dzięki czemu możesz nadal używać klucza pomocniczego, jeśli zdecydujesz się na ponowne wygenerowanie klucza podstawowego, i na odwrót.
* *Klucze zapytań* umożliwiają dostęp tylko do odczytu do indeksów oraz dokumentów i są zazwyczaj dystrybuowane do aplikacji klienckich, które wysyłają żądania wyszukiwania.

Podczas tworzenia indeksu można użyć zarówno podstawowego, jak i pomocniczego klucza administratora.

<a name="CreateSearchServiceClient"></a>

## II. Tworzenie wystąpienia klasy SearchServiceClient
Aby rozpocząć korzystanie z zestawu .NET SDK usługi Azure Search, konieczne jest utworzenie wystąpienia klasy `SearchServiceClient`. Ta klasa ma kilka konstruktorów. Konstruktor odpowiedni w tym przypadku przyjmuje jako parametry nazwę usługi wyszukiwania i obiekt `SearchCredentials`. `SearchCredentials` opakowuje klucz api-key.

Poniższy kod tworzy nową klasę `SearchServiceClient` przy użyciu wartości dla nazwy usługi wyszukiwania i klucza api-key, które są przechowywane w pliku konfiguracji aplikacji (`app.config` lub `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient` ma właściwość `Indexes`. Ta właściwość udostępnia wszystkie metody, które są potrzebne do tworzenia, wyświetlania list, aktualizowania lub usuwania indeksów usługi Azure Search.

> [!NOTE]
> Klasa `SearchServiceClient` zarządza połączeniami z usługą wyszukiwania. W celu uniknięcia otwarcia zbyt wielu połączeń, należy, w miarę możliwości, udostępnić pojedyncze wystąpienie klasy `SearchServiceClient` w aplikacji. Metody tej klasy są bezpieczne wątkowo, co pozwala na tego rodzaju udostępnianie.
> 
> 

<a name="DefineIndex"></a>

## III. Definiowanie indeksu usługi Azure Search przy użyciu klasy `Index`
Pojedyncze wywołanie metody `Indexes.Create` spowoduje utworzenie indeksu. Ta metoda przyjmuje jako parametr obiekt `Index`, który definiuje indeks usługi Azure Search. Należy utworzyć obiekt `Index` i zainicjować go w następujący sposób:

1. Ustaw właściwość `Name` obiektu `Index` na nazwę Twojego indeksu.
2. Ustaw właściwość `Fields` obiektu `Index` na tablicę obiektów `Field`. Każdy z obiektów `Field` definiuje zachowanie pola w Twoim indeksie. Możesz podać nazwę pola konstruktorowi wraz z typem danych (lub analizatorem w przypadku pól ciągów). Możesz również ustawić inne właściwości, takie jak `IsSearchable`, `IsFilterable` itp.

Ważne, aby podczas projektowania indeksu pamiętać o środowisku wyszukiwania oraz o potrzebach biznesowych użytkownika, ponieważ do każdego obiektu `Field` należy przypisać [odpowiednie właściwości](https://msdn.microsoft.com/library/azure/dn798941.aspx). Te właściwości kontrolują, które funkcje wyszukiwania (filtrowanie, tworzenie aspektów, sortowanie, wyszukiwanie pełnotekstowe itp.) dotyczą określonych pól. Dla każdej właściwości, która nie zostanie jawnie ustawiona, klasa `Field` domyślnie wyłączy odpowiednią funkcję wyszukiwania.

W naszym przykładzie indeks ma nazwę „hotels”, a pola zostały zdefiniowane w następujący sposób:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Dla każdego obiektu `Field` starannie wybraliśmy wartości właściwości, kierując się tym, jak naszym zdaniem będą one używane w aplikacji. Na przykład istnieje duże prawdopodobieństwo, że osoby szukające hoteli będzie interesować dopasowanie słów kluczowych w polu `description`, dlatego włączyliśmy wyszukiwanie pełnotekstowe dla tego pola przez ustawienie właściwości `IsSearchable` na wartość `true`.

Zwróć uwagę, że dokładnie jedno pole typu `DataType.String` w Twoim indeksie musi być wyznaczone jako pole *klucza* przez ustawienie właściwości `IsKey` na wartość `true` (zobacz pole `hotelId` w powyższym przykładzie).

Powyższa definicja indeksu używa niestandardowego analizatora języków dla pola `description_fr`, ponieważ jest ono przeznaczone do przechowywania tekstu w języku francuskim. Aby uzyskać więcej informacji o analizatorach języka, zobacz [temat Language support (Obsługa języka) w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn879793.aspx) oraz odpowiadający mu [wpis w blogu](https://azure.microsoft.com/blog/language-support-in-azure-search/).

> [!NOTE]
> Zwróć uwagę, że przekazanie pola `AnalyzerName.FrLucene` w konstruktorze sprawi, że obiekt `Field` przyjmie automatycznie typ `DataType.String`, a właściwość `IsSearchable` zostanie ustawiona na wartość `true`.
> 
> 

## IV. Tworzenie indeksu
Po zainicjowaniu obiektu `Index` możesz utworzyć indeks przez wywołanie metody `Indexes.Create` względem obiektu `SearchServiceClient`:

```csharp
serviceClient.Indexes.Create(definition);
```

W przypadku żądania zakończonego powodzeniem metoda zwróci dane normalnie. W przypadku problemu z żądaniem, takiego jak nieprawidłowy parametr, metoda zgłosi wyjątek `CloudException`.

Gdy ukończysz pracę z indeksem i zechcesz go usunąć, po prostu wywołaj metodę `Indexes.Delete` względem obiektu `SearchServiceClient`. Na przykład indeks „hotele” możesz usunąć w następujący sposób:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Przykład kodu przedstawiony w tym artykule używa metod synchronicznych zestawu .NET SDK usługi Azure Search dla uproszczenia. Zalecamy użycie metod asynchronicznych w aplikacjach, aby pozostały skalowalne i szybko reagowały. W powyższych przykładach można było użyć metod `CreateAsync` i `DeleteAsync` zamiast `Create` i `Delete`.
> 
> 

## Następne kroki
Po utworzeniu indeksu usługi Azure Search można [przekazać zawartość do indeksu](search-what-is-data-import.md), aby rozpocząć wyszukiwanie danych.

<!--HONumber=Sep16_HO3-->


