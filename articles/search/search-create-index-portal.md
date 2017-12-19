---
title: "Tworzenie indeksu (portal — usługi Azure Search) | Dokumentacja firmy Microsoft"
description: "Tworzenie indeksu za pomocą portalu Azure."
services: search
manager: jhubbard
author: heidisteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: a7d98ab0937a7d3f932d5df34c19ae091129804e
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Tworzenie indeksu usługi Azure Search przy użyciu portalu Azure
> [!div class="op_single_selector"]
> * [Omówienie](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

W portalu Azure, aby prototypu za pomocą projektanta wbudowanych indeks lub Utwórz [indeksu wyszukiwania](search-what-is-an-index.md) do uruchamiania w usłudze Azure Search. 

## <a name="prerequisites"></a>Wymagania wstępne

W tym artykule przyjęto [subskrypcji platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) i [usługi Azure Search](search-create-service-portal.md).  

## <a name="find-your-search-service"></a>Znajdowanie usługi wyszukiwania
1. Zaloguj się do strony portalu systemu Azure i przejrzyj [wyszukiwania usługi dla subskrypcji](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Wybierz swoją usługę Azure Search.

## <a name="name-the-index"></a>Nazwa indeksu

1. Kliknij przycisk **Dodaj indeks** przycisku w pasku poleceń u góry strony.
2. Nazwa indeksu usługi Azure Search. 
   * Zaczynać się literą.
   * Użyj tylko małe litery, cyfry i łączniki ("-").
   * Ogranicz nazwę do 60 znaków.

  Nazwa indeksu staje się częścią adresu URL punktu końcowego używane w przypadku połączeń do indeksu i wysyłania żądań HTTP w interfejsu API REST usługi Azure Search.

## <a name="define-the-fields-of-your-index"></a>Definiowanie pól indeksu

Skład indeksu obejmuje *pola kolekcji* definiuje danych z możliwością wyszukiwania w indeksie. W szczególności określa struktury dokumentów, które można przekazać oddzielnie. Kolekcja pól zawiera wymagane i opcjonalne pola o nazwie i wypełniana atrybuty indeksu, aby określić, jak można użyć pola.

1. W **Dodawanie indeksu** bloku, kliknij przycisk **pola >** Aby wysunąć blok definicji pola. 

2. Zaakceptuj wygenerowany *klucza* pola typu typem Edm.String. Domyślnie pole o nazwie *identyfikator* , ale można ją zmienić, tak długo, jak spełnia ciąg [reguły nazewnictwa](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Pole klucza jest wymagana dla każdego indeksu usługi Azure Search i musi być ciągiem.

3. Dodaj pola, aby w pełni określić dokumenty, które możesz przekazać. Jeśli dokumenty składają się z *identyfikator*, *nazwa hoteli*, *adres*, *miasta*, i *region*, utworzyć odpowiednie pole dla każdej z nich w indeksie. Przegląd [projektowania wskazówki zawarte w poniższej sekcji](#design) pomocy podczas ustawiania atrybutów.

4. Opcjonalnie dodaj pola, które są używane wewnętrznie w wyrażeniach filtru. Można ustawić atrybutów pola do wykluczenia pola z operacji wyszukiwania.

5. Gdy skończysz, kliknij przycisk **OK** zapisu i tworzenia indeksu.

## <a name="tips-for-adding-fields"></a>Porady dotyczące — Dodawanie pól

Tworzenie indeksu w portalu jest znacznym klawiatury. Wykonując ten przepływ pracy, należy zminimalizować kroki:

1. Najpierw należy utworzyć w oknie Lista pól, wprowadzając nazwy i ustawianie typów danych.

2. Następnie użyj pól wyboru w górnej części każdego atrybutu w celu zbiorczego ustawienie dla wszystkich pól i selektywnie wyczyścić pola kilka pól, które nie wymagają. Na przykład ciąg pola są zazwyczaj można wyszukiwać. Tak, można kliknąć **pobieranie** i **wyszukiwanie** zarówno zwracać wartości pola w wynikach wyszukiwania, a także umożliwia wyszukiwanie pełnotekstowe pola. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Wskazówki dotyczące projektowania do ustawiania atrybutów

Chociaż w dowolnym momencie można dodawać nowe pola, istniejące definicje pól są zablokowane na indeks przez czas ich istnienia. Z tego powodu Deweloperzy zazwyczaj korzystanie z portalu Tworzenie prostego indeksów, testy pomysłów lub za pomocą portalu stron do odszukania ustawienie. Częste iteracji w projekt indeksu jest bardziej wydajne, jeśli wykonujesz podejście oparte na kodzie, dzięki czemu można łatwo odtworzyć indeksu.

Analizatory i sugestorów są skojarzone z polami przed zapisaniem indeksu. Należy kliknąć przycisk za pośrednictwem każdej karty stronę, aby dodać analizatorów języka lub sugestorów do definicji indeksu.

Pól ciągów często są oznaczone jako **wyszukiwanie** i **pobieranie**.

Pola używane, aby zawęzić wyniki wyszukiwania zawierają **sortowanie**, **Filterable**, i **aspektów**.

Atrybuty pól określają, jak używane pola, np. czy jest używana w wyszukiwania pełnotekstowego, nawigacji aspektowej, sortowanie i tak dalej. W poniższej tabeli opisano każdy atrybut.

|Atrybut|Opis|  
|---------------|-----------------|  
|**Wyszukiwanie**|Pełnotekstowe wyszukiwanie może ulec leksykalne analizy, takich jak dzielenia wyrazów podczas indeksowania. Jeśli ustawisz wyszukiwanie pola wartości, takich jak "day słoneczna" wewnętrznie go zostanie podzielony na poszczególnych tokeny "Słoneczna" i "day". Aby uzyskać więcej informacji, zobacz [jak Pełna działa wyszukiwania tekstu](search-lucene-query-architecture.md).|  
|**Filtrowanie**|Zawartymi w **$filter** zapytania. Można filtrować polami typu `Edm.String` lub `Collection(Edm.String)` nie zostały poddane dzielenia wyrazów, więc porównania dla tylko dokładne dopasowania. Na przykład jeśli ustawisz takiego pola f "słoneczna dzień" `$filter=f eq 'sunny'` znajdziesz żadnych wyników, ale `$filter=f eq 'sunny day'` będzie. |  
|**Sortowanie**|Domyślnie system sortujące wyniki według wyników, ale można skonfigurować sortowania na podstawie pól w dokumentach. Pola typu `Collection(Edm.String)` nie może być **sortowanie**. |  
|**Tworzenie aspektów**|Zwykle używanych w prezentacji wyników wyszukiwania, która zawiera liczby trafień według kategorii (na przykład hotele w określonym mieście). Nie można użyć tej opcji z polami typu `Edm.GeographyPoint`. Pola typu `Edm.String` , które są **filtrowanie**, **sortowanie**, lub **aspektów** może mieć maksymalnie 32 kilobajty długości. Aby uzyskać więcej informacji, zobacz [Create Index (interfejsu API REST)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**klucz**|Unikatowy identyfikator dla dokumentów w indeksie. Należy wybrać dokładnie jedno pole jako pole klucza i musi być typu `Edm.String`.|  
|**Pobieranie**|Określa, czy pole może być zwracany w wynikach wyszukiwania. Jest to przydatne, jeśli chcesz użyć pola (takich jak *marża*) jako filtru, sortowania, lub oceniania mechanizmu, ale nie ma pola, które mają być widoczne dla użytkownika końcowego. Ten atrybut musi być `true` dla `key` pola.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>Tworzenie indeksu hotels używane w sekcjach interfejsu API przykład

Azure dokumentacji interfejsu API Search przykłady kodu, oferujący funkcje prostą *hotele* indeksu. Poniższe zrzuty ekranu, zawiera definicję indeksu, łącznie z analizatora języka francuskiego, określona w definicji indeksu, który można odtworzyć jako ćwiczenia, w portalu.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Następne kroki

Po utworzeniu indeksu usługi Azure Search, można przenieść do następnego kroku: [przekazywanie danych z możliwością wyszukiwania w indeksie](search-what-is-data-import.md).

Alternatywnie możesz można również Przyjrzyjmy się głębiej indeksów. Oprócz kolekcji pól Indeks określa również analizatory, sugestorów oceniania profile i ustawienia specyfikacji CORS. Portal zawiera karty do definiowania często spotykanych elementów: pola, analizatory i sugestorów. Aby utworzyć lub zmodyfikować inne elementy, można użyć interfejsu API REST lub zestawu .NET SDK.

## <a name="see-also"></a>Zobacz też

 [Jak działa wyszukiwanie pełnotekstowe](search-lucene-query-architecture.md)  
 [Usługi interfejsu API REST wyszukiwania](https://docs.microsoft.com/rest/api/searchservice/) [zestawu .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

